# Setting Up Automation<a name="automation-setup"></a>

Setting up Automation requires that you verify user access to the Automation service and configure roles so that the service can perform actions on your instances\. Optionally, you can also configure Automation to send events to Amazon CloudWatch Events\. You can also configure an Automation workflow to run when a specific CloudWatch event occurs\. This is called specifying Automation as the *target* of a CloudWatch event\.


+ [Configuring Access for Systems Manager Automation](#automation-setup-user)
+ [Configuring CloudWatch Events for Systems Manager Automation \(Optional\)](automation-cwe.md)
+ [Configuring Automation as a CloudWatch Events Target \(Optional\)](automation-cwe-target.md)

## Configuring Access for Systems Manager Automation<a name="automation-setup-user"></a>

Configuring access to Systems Manager Automation requires that you complete the following tasks\.

1. **Verify user access**: Verify that you have permission to run Automation workflows\. If your AWS Identity and Access Management \(IAM\) user account, group, or role is assigned administrator permissions, then you have access to Systems Manager Automation\. If you don't have administrator permissions, then an administrator must give you permission by assigning the **AmazonSSMFullAccess** managed policy, or a policy that provides comparable permissions, to your IAM account, group, or role\.

1. **Configure instance access by creating and assigning an instance profile role \(Required\)**: Each instance that runs an Automation workflow requires an IAM instance profile role\. This role gives Automation permission to perform actions on your instances, such as executing commands or starting and stopping services\. If you previously created an instance profile role for Systems Manager, as described in [Task 2: Create an Instance Profile Role for Systems Manager](systems-manager-access.md#sysman-configuring-access-role) in the **Configuring Access to Systems Manager** topic, then you can use this same instance profile role for Automation\. For information about how to attach this role to an existing instance, see [Attaching an IAM Role to an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide*\.

**Note**  
Automation previously required that you specify a service role \(or *assume* role\) so that the service had permission to perform actions on your behalf\. Automation no longer requires this role because the service now operates by using the context of the user who invoked the execution\.   
However, the following situations still require that you specify a service role for Automation:  
When you want to restrict a user's privileges on a resource, but you want the user to execute an Automation workflow that requires higher privileges\. In this scenario, you can create a service role with higher privileges and allow the user to execute the workflow\.
Operations that you expect to run longer than 12 hours require a service role\.

If you need to create a service role and an instance profile role for Automation, you can use one of the following methods\.


+ [Method 1: Using AWS CloudFormation to Configure Roles for Automation](#automation-cf)
+ [Method 2: Using IAM to Configure Roles for Automation](#automation-permissions)

### Method 1: Using AWS CloudFormation to Configure Roles for Automation<a name="automation-cf"></a>

You can create an IAM instance profile role and a service role for Automation from an AWS CloudFormation template\.

After you create the instance profile role, you must assign it to any instance that you plan to configure using Automation\. For information about how to assign the role to an existing instance, see [Attaching an IAM Role to an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide*\. For information about how to assign the role when you create a new instance, see [Task 3: Create an Amazon EC2 Instance that Uses the Systems Manager Role](systems-manager-access.md#sysman-create-instance-with-role) in the **Configuring Access to Systems Manager** topic\.

**Note**  
You can also use these roles and their Amazon Resource Names \(ARNs\) in Automation documents, such as the AWS\-UpdateLinuxAmi document\. Using these roles or their ARNs in Automation documents enables Automation to perform actions on your managed instances, launch new instances, and perform actions on your behalf\. To view an example, see [Automation CLI Walkthrough: Patch a Linux AMI](automation-cliwalk.md)\.

#### Create the Instance Profile Role and Service Role Using AWS CloudFormation<a name="automation-cf-create"></a>

Use the following procedure to create the required IAM roles for Systems Manager Automation by using AWS CloudFormation\.

**To create the required IAM roles**

1. Choose the **Launch Stack** button\. The button opens the AWS CloudFormation console and populates the **Specify an Amazon S3 template URL** field with the URL to the Systems Manager Automation template\.
**Note**  
Choose **View** to view the template\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/automation-setup.html)

1. Choose **Next**\.

1. On the **Specify Details** page, in the **Stack Name** field, either choose to keep the default value or specify your own value\. Choose **Next**\.

1. On the **Options** page, you don’t need to make any selections\. Choose **Next**\.

1. On the **Review** page, scroll down and choose the **I acknowledge that AWS CloudFormation might create IAM resources** option\.

1. Choose **Create**\.

AWS CloudFormation shows the **CREATE\_IN\_PROGRESS** status for approximately three minutes\. The status changes to **CREATE\_COMPLETE** after the stack has been created and your roles are ready to use\.

#### Copying Role Information for Automation<a name="automation-cf-copy"></a>

Use the following procedure to copy information about the instance profile role and Automation service role from the AWS CloudFormation console\. You must specify these roles when you when you run an Automation document\.

**Note**  
You do not need to copy role information using this procedure if you run the AWS\-UpdateLinuxAmi or AWS\-UpdateWindowsAmi documents\. These documents already have the required roles specified as default values\. The roles specified in these documents use IAM managed policies\. 

**To copy the role names**

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Choose the check\-box beside the Automation stack you created in the previous procedure\.

1. Choose the **Resources** tab\.

1. The **Resources** table includes three items in the **Logical ID** column: **AutomationServiceRole**, **ManagedInstanceProfile**, and **ManagedInstanceRole**\.

1. Copy the **Physical ID** for **ManagedInstanceProfile**\. The physical ID will be similar to **Automation\-ManagedInstanceProfile\-1a2b3c4**\. This is the name of your instance profile role\.

1. Paste the instance profile role into a text file to use later\.

1. Choose the **Physical ID** link for **AutomationServiceRole**\. The IAM console opens to a summary of the Automation Service Role\.

1. Copy the Amazon Resource Name \(ARN\) beside **Role ARN**\. The ARN is similar to the following: arn:aws:iam::12345678:role/Automation\-AutomationServiceRole\-1A2B3C4D5E 

1. Paste the ARN into a text file to use later\.

You have finished configuring the required roles for Automation\. You can now use the instance profile role and the Automation service role ARN in your Automation documents\. For more information, see [Automation Console Walkthrough: Patch a Linux AMI](automation-consolewalk.md) and [Automation CLI Walkthrough: Patch a Linux AMI](automation-cliwalk.md)\.

### Method 2: Using IAM to Configure Roles for Automation<a name="automation-permissions"></a>

Configuring access to Systems Manager Automation requires that you complete the following tasks\.

1. **Verify user access**: Verify that you have permission to run Automation workflows\. If your AWS Identity and Access Management \(IAM\) user account, group, or role is assigned administrator permissions, then you have access to Systems Manager Automation\. If you don't have administrator permissions, then an administrator must give you permission by assigning the **AmazonSSMFullAccess** managed policy, or a policy that provides comparable permissions, to your IAM account, group, or role\.

1. **Configure instance access by creating and assigning an instance profile role \(Required\)**: Each instance that runs an Automation workflow requires an IAM instance profile role\. This role gives Automation permission to perform actions on your instances, such as executing commands or starting and stopping services\. If you previously created an instance profile role for Systems Manager, as described in [Task 2: Create an Instance Profile Role for Systems Manager](systems-manager-access.md#sysman-configuring-access-role) in the **Configuring Access to Systems Manager** topic, then you can use this same instance profile role for Automation\. For information about how to attach this role to an existing instance, see [Attaching an IAM Role to an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide*\.

**Note**  
Automation previously required that you specify a service role \(or *assume* role\) so that the service had permission to perform actions on your behalf\. Automation no longer requires this role because the service now operates by using the context of the user who invoked the execution\.   
However, the following situations still require that you specify a service role for Automation:  
When you want to restrict a user's privileges on a resource, but you want the user to execute an Automation workflow that requires higher privileges\. In this scenario, you can create a service role with higher privileges and allow the user to execute the workflow\.
Operations that you expect to run longer than 12 hours require a service role\.

If you need to create an instance profile role and a service role for Systems Manager Automation, complete the following tasks\.


+ [Task 1: Create a Service Role for Automation](#automation-role)
+ [Task 2: Add a Trust Relationship for Automation](#automation-trust2)
+ [Task 3: Attach the iam:PassRole Policy to Your Automation Role](#automation-passpolicy)
+ [Task 4: Configure User Access to Automation](#automation-passrole)
+ [Task 5: Create an Instance Profile Role](#automation-instance-role)

#### Task 1: Create a Service Role for Automation<a name="automation-role"></a>

Use the following procedure to create a service role \(or *assume* role\) for Systems Manager Automation\.

**Note**  
You can also use these roles and their Amazon Resource Names \(ARNs\) in Automation documents, such as the AWS\-UpdateLinuxAmi document\. Using these roles or their ARNs in Automation documents enables Automation to perform actions on your managed instances, launch new instances, and perform actions on your behalf\. To view an example, see [Automation CLI Walkthrough: Patch a Linux AMI](automation-cliwalk.md)\.

**To create an IAM role and allow Automation to assume it**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Select type of trusted entity** page, under **AWS Service**, choose **EC2**\.

1. In the **Select your use case** section, choose **EC2**, and then choose **Next: Permissions**\.

1. On the **Attached permissions policy** page, search for the **AmazonSSMAutomationRole** policy, choose it, and then choose **Next: Review**\. 

1. On the **Review** page, type a name in the **Role name** box, and then type a description\.

1. Choose **Create role**\. The system returns you to the **Roles** page\.

1. On the **Roles** page, choose the role you just created to open the **Summary** page\. Make a note of the **Role Name** and **Role ARN**\. You will specify the role ARN when you attach the iam:PassRole policy to your IAM account in the next procedure\. You can also specify the role name and the ARN in Automation documents\.

   Leave the **Summary** page open\. 

**Note**  
The AmazonSSMAutomationRole policy assigns the Automation role permission to a subset of AWS Lambda functions within your account\. These functions begin with "Automation"\. If you plan to use Automation with Lambda functions, the Lambda ARN must use the following format:  

```
"arn:aws:lambda:*:*:function:Automation*"
```
If you have existing Lambda functions whose ARNs do not use this format, then you must also attach an additional Lambda policy to your automation role, such as the **AWSLambdaRole** policy\. The additional policy or role must provide broader access to Lambda functions within the AWS account\.

#### Task 2: Add a Trust Relationship for Automation<a name="automation-trust2"></a>

Use the following procedure to configure the service role policy to trust Automation\.

**To add a trust relationship for Automation**

1. In the **Summary** page for the role you just created, choose the **Trust Relationships** tab, and then choose **Edit Trust Relationship**\.

1. Add "ssm\.amazonaws\.com", as shown in the following example:

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Sid":"",
            "Effect":"Allow",
            "Principal":{
               "Service":[
                  "ec2.amazonaws.com",
                  "ssm.amazonaws.com"
               ]
            },
            "Action":"sts:AssumeRole"
         }
      ]
   }
   ```

1. Choose **Update Trust Policy**\.

1. Leave the **Summary** page open\. 

#### Task 3: Attach the iam:PassRole Policy to Your Automation Role<a name="automation-passpolicy"></a>

Use the following procedure to attach the iam:PassRole policy to your Automation service role\. This enables the Automation service to pass the role to other services or Systems Manager capabilities when running Automation workflows\.

**To attach the iam:PassRole policy to your Automation role**

1. In the **Summary** page for the role you just created, choose the **Permissions** tab\.

1. Choose **Add inline policy**\.

1. On the **Create policy** page, choose the **Visual editor** tab\.

1. Choose **Service**, and then choose **IAM**\.

1. Choose **Select actions**\.

1. In the **Filter actions** text box, type PassRole, and then choose the **PassRole** option\.

1. Choose **Resources**\. Verify that **Specific** is selected, and then choose **Add ARN**\.

1. In the **Specify ARN for role** field, paste the Automation role ARN that you copied at the end of Task 1\. The system autopopulates the **Account** and **Role name with path** fields\.

1. Choose **Add**\.

1. Choose **Review policy**\.

1. On the **Review Policy** page, type a name and then choose **Create Policy**\.

#### Task 4: Configure User Access to Automation<a name="automation-passrole"></a>

If your AWS Identity and Access Management \(IAM\) user account, group, or role is assigned administrator permissions, then you have access to Systems Manager Automation\. If you don't have administrator permissions, then an administrator must give you permission by assigning the **AmazonSSMFullAccess** managed policy, or a policy that provides comparable permissions, to your IAM account, group, or role\.

Use the following procedure to configure a user account to use Automation\. The user account you choose will have permission to configure and execute Automation\. If you need to create a new user account, see [Creating an IAM User in Your AWS Account](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html) in the *IAM User Guide*\.

**To configure user access and attach the iam:PassRole policy to a user account**

1. In the IAM navigation pane, choose **Users**, and then choose the user account you want to configure\.

1. On the **Permissions** tab, in the policies list, verify that either the **AmazonSSMFullAccess** policy is listed or there is a comparable policy that gives the account permissions to access Systems Manager\.

1. Choose **Add inline policy**\.

1. On the **Set Permissions** page, choose **Policy Generator**, and then choose **Select**\.

1. Verify that **Effect** is set to **Allow**\.

1. From **AWS Services**, choose **AWS Identity and Access Management**\.

1. From **Actions**, choose **PassRole**\.

1. In the **Amazon Resource Name \(ARN\)** field, paste the ARN for the Automation service role you copied at the end of Task 1\.

1. Choose **Add Statement**, and then choose **Next Step**\.

1. On the **Review Policy** page, choose **Apply Policy**\.

#### Task 5: Create an Instance Profile Role<a name="automation-instance-role"></a>

Each instance that runs an Automation workflow requires an IAM instance profile role\. This role gives Automation permission to perform actions on your instances, such as executing commands or starting and stopping services\. If you previously created an instance profile role for Systems Manager, as described in [Task 2: Create an Instance Profile Role for Systems Manager](systems-manager-access.md#sysman-configuring-access-role) in the **Configuring Access to Systems Manager** topic, then you can use this same instance profile role for Automation\. If you have not created an instance profile role as described in that topic, please do so now\. For information about how to attach this role to an existing instance, see [Attaching an IAM Role to an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide*\.

You have finished configuring the required roles for Automation\. You can now use the instance profile role and the Automation service role ARN in your Automation documents\. For more information, see [Automation Console Walkthrough: Patch a Linux AMI](automation-consolewalk.md) and [Automation CLI Walkthrough: Patch a Linux AMI](automation-cliwalk.md)\.