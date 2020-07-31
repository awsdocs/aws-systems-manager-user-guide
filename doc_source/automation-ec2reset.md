# Walkthrough: Reset passwords and SSH keys on EC2 instances<a name="automation-ec2reset"></a>

You can use the **AWSSupport\-ResetAccess** document to automatically reenable local Administrator password generation on EC2 instances for Windows Server, and to generate a new SSH key on EC2 instances for Linux\. The **AWSSupport\-ResetAccess** document is designed to perform a combination of Systems Manager actions, AWS CloudFormation actions, and Lambda functions that automate the steps normally required to reset the local administrator password\.

You can use Automation with the **AWSSupport\-ResetAccess** document to solve the following problems:

**Windows**

*You lost the EC2 key pair*: To resolve this problem, you can use the **AWSSupport\-ResetAccess** document to create a password\-enabled AMI from your current instance, launch a new instance from the AMI, and select a key pair you own\.

*You lost the local Administrator password*: To resolve this problem, you can use the **AWSSupport\-ResetAccess** document to generate a new password that you can decrypt with the current EC2 key pair\.

**Linux**

*You lost your EC2 key pair, or you configured SSH access to the instance with a key you lost*: To resolve this problem, you can use the **AWSSupport\-ResetAccess** document to create a new SSH key for your current instance, which enables you to connect to the instance again\.

**Note**  
If your v for Windows Server is configured for Systems Manager, you can also reset your local Administrator password by using EC2Rescue and Run Command\. For more information, see [Using EC2Rescue for Windows Server with Systems Manager Run Command](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2rw-ssm.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## How it works<a name="automation-ec2reset-how"></a>

Troubleshooting an instance with Automation and the **AWSSupport\-ResetAccess** document works as follows:
+ You specify the ID of the instance and run the Automation workflow\.
+ The system creates a temporary VPC, and then runs a series of Lambda functions to configure the VPC\.
+ The system identifies a subnet for your temporary VPC in the same Availability Zone as your original instance\.
+ The system launches a temporary, SSM\-enabled helper instance\.
+ The system stops your original instance, and creates a backup\. It then attaches the original root volume to the helper instance\.
+ The system uses Run Command to run EC2Rescue on the helper instance\. On Windows, EC2Rescue enables password generation for the local Administrator by using EC2Config or EC2Launch on the attached, original root volume\. On Linux, EC2Rescue generates and injects a new SSH key and saves the private key, encrypted, in Parameter Store\. When finished, EC2Rescue reattaches the root volume back to the original instance\.
+ The system creates a new Amazon Machine Image \(AMI\) of your instance, now that password generation is enabled\. You can use this AMI to create a new EC2 instance, and associate a new key pair if needed\.
+ The system restarts your original instance, and terminates the temporary instance\. The system also terminates the temporary VPC and the Lambda functions created at the start of the automation\.
+ **Windows**: Your instance generates a new password you can decode from the Amazon EC2 console using the current key pair assigned to the instance\.

  **Linux**: You can SSH to the instance by using the SSH key stored in Systems Manager Parameter Store as **/ec2rl/openssh/*instance\_id*/key**\.

## Before you begin<a name="automation-ec2reset-begin"></a>

Before you run the following Automation, do the following:
+ Copy the instance ID of the instance on which you want to reset the Administrator password\. You will specify this ID in the procedure\.
+ Optionally, collect the ID of a subnet in the same availability zone as your unreachable instance\. The EC2Rescue instance will be created in this subnet\. If you don’t specify a subnet, then Automation creates a new temporary VPC in your AWS account\. Verify that your AWS account has at least one VPC available\. By default, you can create five VPCs in a Region\. If you already created five VPCs in the Region, the automation fails without making changes to your instance\. For more information about Amazon VPC quotas, see [VPC and Subnets](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html#vpc-limits-vpcs-subnets) in the *Amazon VPC User Guide*\.
+ Optionally, you can create and specify an AWS Identity and Access Management \(IAM\) role for Automation\. If you don't specify this role, then Automation runs in the context of the user who ran the automation\. For more information about creating roles for Automation, see [Running an automation by using an IAM service role](automation-walk-security-assume.md)\.

### Granting AWSSupport\-EC2Rescue permissions to perform actions on your instances<a name="automation-ec2reset-access"></a>

EC2Rescue needs permission to perform a series of actions on your instances during the Automation execution\. These actions invoke the AWS Lambda, IAM, and Amazon EC2 services to safely and securely attempt to remediate issues with your instances\. If you have Administrator\-level permissions in your AWS account and/or VPC, you might be able to run the automation without configuring permissions, as described in this section\. If you don't have Administrator\-level permissions, then you or an administrator must configure permissions by using one of the following options\.
+ [Granting permissions by using IAM policies](#automation-ec2reset-access-iam)
+ [Granting permissions by using an AWS CloudFormation template](#automation-ec2reset-access-cfn)

#### Granting permissions by using IAM policies<a name="automation-ec2reset-access-iam"></a>

You can either attach the following IAM policy to your IAM user account, group, or role as an inline policy; or, you can create a new IAM managed policy and attach it to your user account, group, or role\. For more information about adding an inline policy to your user account, group, or role see [Working With Inline Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_inline-using.html)\. For more information about creating a new managed policy, see [Working With Managed Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-using.html)\.

**Note**  
If you create a new IAM managed policy, you must also attach the **AmazonSSMAutomationRole** managed policy to it so that your instances can communicate with the Systems Manager API\.

**IAM Policy for AWSSupport\-ResetAccess**

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Action": [
            "lambda:InvokeFunction",
            "lambda:DeleteFunction",
            "lambda:GetFunction"
         ],
         "Resource": "arn:aws:lambda:*:aws-account-id:function:AWSSupport-EC2Rescue-*",
         "Effect": "Allow"
      },
      {
         "Action": [
            "s3:GetObject",
            "s3:GetObjectVersion"
         ],
         "Resource": [
            "arn:aws:s3:::awssupport-ssm.*/*.template",
            "arn:aws:s3:::awssupport-ssm.*/*.zip"
         ],
         "Effect": "Allow"
      },
      {
         "Action": [
            "iam:CreateRole",
            "iam:CreateInstanceProfile",
            "iam:GetRole",
            "iam:GetInstanceProfile",
            "iam:PutRolePolicy",
            "iam:DetachRolePolicy",
            "iam:AttachRolePolicy",
            "iam:PassRole",
            "iam:AddRoleToInstanceProfile",
            "iam:RemoveRoleFromInstanceProfile",
            "iam:DeleteRole",
            "iam:DeleteRolePolicy",
            "iam:DeleteInstanceProfile"
         ],
         "Resource": [
            "arn:aws:iam::aws-account-id:role/AWSSupport-EC2Rescue-*",
            "arn:aws:iam::aws-account-id:instance-profile/AWSSupport-EC2Rescue-*"
         ],
         "Effect": "Allow"
      },
      {
         "Action": [
            "lambda:CreateFunction",
            "ec2:CreateVpc",
            "ec2:ModifyVpcAttribute",
            "ec2:DeleteVpc",
            "ec2:CreateInternetGateway",
            "ec2:AttachInternetGateway",
            "ec2:DetachInternetGateway",
            "ec2:DeleteInternetGateway",
            "ec2:CreateSubnet",
            "ec2:DeleteSubnet",
            "ec2:CreateRoute",
            "ec2:DeleteRoute",
            "ec2:CreateRouteTable",
            "ec2:AssociateRouteTable",
            "ec2:DisassociateRouteTable",
            "ec2:DeleteRouteTable",
            "ec2:CreateVpcEndpoint",
            "ec2:DeleteVpcEndpoints",
            "ec2:ModifyVpcEndpoint",
            "ec2:Describe*"
         ],
         "Resource": "*",
         "Effect": "Allow"
      }
   ]
}
```

#### Granting permissions by using an AWS CloudFormation template<a name="automation-ec2reset-access-cfn"></a>

AWS CloudFormation automates the process of creating IAM roles and policies by using a preconfigured template\. Use the following procedure to create the required IAM roles and policies for the EC2Rescue Automation by using AWS CloudFormation\.

**To create the required IAM roles and policies for EC2Rescue**

1. Download [samples/AWSSupport-EC2RescueRole.zip](samples/AWSSupport-EC2RescueRole.zip) and extract the `AWSSupport-EC2RescueRole.json` file to a directory on your local machine\.

1. If your AWS account is in a special partition, edit the template to change the ARN values to those for your partition\.

   For example, for the China Regions, change all cases of `arn:aws` to `arn:aws-cn`\.

1. Sign in to the AWS Management Console and open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Choose **Create stack**, **With new resources \(standard\)**\.

1. On the **Create stack** page, for **Prerequisite \- Prepare template**, choose **Template is ready**\.

1. For **Specify template**, choose **Upload a template file**\.

1. Choose **Choose file**, and then browse to and select the `AWSSupport-EC2RescueRole.json` file from the directory where you extracted it\.

1. Choose **Next**\.

1. On the **Specify stack details** page, for **Stack name** field, enter a name to identify this stack, and then choose **Next**\.

1. \(Optional\) In the **Tags** area, apply one or more tag key name/value pairs to the stack\.

   Tags are optional metadata that you assign to a resource\. Tags enable you to categorize a resource in different ways, such as by purpose, owner, or environment\. For example, you might want to tag a stack to identify the type of tasks it runs, the types of targets or other resources involved, and the environment it runs in\.

1. Choose **Next**

1. On the **Review** page, review the stack details, and then scroll down and choose the **I acknowledge that AWS CloudFormation might create IAM resources** option\.

1. AWS CloudFormation shows the **CREATE\_IN\_PROGRESS** status for a few minutes\. The status changes to **CREATE\_COMPLETE** after the stack has been created\. You can also choose the refresh icon to check the status of the create process\.

1. In the stack list, choose the option next to the stack you just created, and then choose the **Outputs** tab\.

1. Copy the **Value**\. The is the ARN of the AssumeRole\. You will specify this ARN when you run the Automation\. 

## Running the Automation<a name="automation-ec2reset-executing"></a>

The following procedure describes how to run the **AWSSupport\-ResetAccess** document by using the AWS Systems Manager console\.

**Important**  
The following Automation execution stops the instance\. Stopping the instance can result in lost data on attached instance store volumes \(if present\)\. Stopping the instance can also cause the public IP to change, if no Elastic IP is associated\. To avoid these configuration changes, use Run Command to reset access\. For more information, see [Using EC2Rescue for Windows Server with Systems Manager Run Command](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2rw-ssm.html) in the *Amazon EC2 User Guide for Windows Instances*\.

**To run the AWSSupport\-ResetAccess Automation**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Automation**\.

1. Choose **Execute automation**\.

1. In the **Automation document** section, choose **Owned by Amazon** from the list\.

1. In the documents list, choose the button in the card for **AWSSupport\-ResetAccess**, and then choose **Next**\.

1. In the **Execute automation document** page, choose **Simple execution**\.

1. In the **Document details** section, verify that **Document version** is set to the highest default version\. For example, **$DEFAULT** or **3 \(default\)**\.

1. In the **Input parameters** section, specify the following parameters: 

   1. For **InstanceID**, specify the ID of the unreachable instance\. 

   1. For **SubnetId**, specify a subnet in an existing VPC in the same availability zone as the instance you specified\. By default, Systems Manager creates a new VPC, but you can specify a subnet in an existing VPC if you want\.
**Note**  
If you don't see the option to specify a subnet ID, verify that you are using the latest **Default** version of the document\.

   1. For **EC2RescueInstanceType**, specify an instance type for the EC2Rescue instance\. The default instance type is `t2.small`\.

   1. For **AssumeRole**, if you created roles for this Automation by using the AWS CloudFormation procedure described earlier in this topic, then specify the AssumeRole ARN that you noted in the AWS CloudFormation console\.

1. \(Optional\) In the **Tags** area, apply one or more tag key name/value pairs to help identify the execution, for example `Key=Purpose,Value=ResetAccess`\.

1. Choose **Execute**\.

1. To monitor the execution progress, choose the running Automation workflow, and then choose the **Steps** tab\. When the execution is finished, choose the **Descriptions** tab, and then choose **View output** to view the results\. To view the output of individual steps, choose the **Steps** tab, and then choose **View Outputs** next to a step\.

The Automation creates a backup AMI and a password\-enabled AMI as part of the workflow\. All other resources created by the Automation workflow are automatically deleted, but these AMIs remain in your account\. The AMIs are named using the following conventions:
+ Backup AMI: `AWSSupport-EC2Rescue:InstanceId`
+ Password\-enabled AMI: AWSSupport\-EC2Rescue: Password\-enabled AMI from *InstanceId*

You can locate these AMIs by searching on the Automation execution ID\.

For Linux, the new SSH private key for your instance is saved, encrypted, in Parameter Store\. The parameter name is **/ec2rl/openssh/*instance\_id*/key**\.