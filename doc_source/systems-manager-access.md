# Configuring Access to Systems Manager<a name="systems-manager-access"></a>

Complete the following tasks to configure access for AWS Systems Manager\.

**Note**  
For more information about access permissions for Systems Manager, see [Authentication and Access Control for AWS Systems Manager](auth-and-access-control.md)\.


+ [Task 1: Configure User Access for Systems Manager](#sysman-access-user)
+ [Task 2: Create an Instance Profile Role for Systems Manager](#sysman-configuring-access-role)
+ [Task 3: Create an Amazon EC2 Instance that Uses the Systems Manager Role](#sysman-create-instance-with-role)
+ [Optional Access Configurations](#sysman-create-iam)

## Task 1: Configure User Access for Systems Manager<a name="sysman-access-user"></a>

If your IAM user account, group, or role is assigned administator permissions, then you have access to Systems Manager\. You can skip this task\. If you don't have administrator permissions, then an administrator must update your IAM user account, group, or role to include the following permissions:

+ **To access Resource Groups**: You must add the `resource-groups:*` permissions entity to your IAM user account, group, or role\. For more information, see [Setting Up Permissions](https://docs.aws.amazon.com/ARG/latest/userguide/gettingstarted-prereqs.html#rg-permissions) in the *AWS Resource Groups* user guide\.

+ **To access Insights**: You must add the following managed policies to your user account, group, or role:

  + AWSHealthFullAccess

  + AWSConfigUserAccess

  + CloudWatchReadOnlyAccess
**Note**  
Access to Inventory and Compliance are covered by the next policies\.

+ **To access Actions and Shared Resources**: You must add either the AmazonSSMFullAccess policy or the AmazonSSMReadOnlyAccess policy\.

For information about how to change permissions for an IAM user account, group, or role, see [Changing Permissions for an IAM User](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) in the *IAM User Guide*\.

## Task 2: Create an Instance Profile Role for Systems Manager<a name="sysman-configuring-access-role"></a>

By default, Systems Manager doesn't have permission to perform actions on your instances\. You must enable access by creating an IAM instance profile role, as described here\. After you create the role, you can assign it to existing instances, or you can create new instances that uses this role, as described in Task 3\.

**Note**  
If you are configuring servers or virtual machines \(VMs\) in a hybrid environment for Systems Manager, you don't need to create the instance profile role\. Instead, you must configure your servers and VMs to use an IAM service role\. For more information, see [Create an IAM Service Role](systems-manager-managedinstances.md#sysman-service-role)\.

**To create an instance profile role for Systems Manager managed instances**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Select type of trusted entity** page, under **AWS Service**, choose **EC2**\.

1. In the **Select your use case** section, choose **EC2 Role for Simple Systems Manager**, and then choose **Next: Permissions**\.

1. On the **Attached permissions policy** page, verify that **AmazonEC2RoleforSSM** is listed, and then choose **Next: Review**\. 

1. On the **Review** page, type a name in the **Role name** box, and then type a description\.
**Note**  
Make a note of the role name\. You will choose this role when you create new instances that you want to manage by using Systems Manager\.

1. Choose **Create role**\. The system returns you to the **Roles** page\.

**Note**  
If you change the IAM instance profile role, then you must either restart SSM Agent or restart the instance\. If you don't, SSM Agent can fail to process requests\.
This procedure created a new role from a pre\-existing IAM policy or *managed policy*\. If you choose to create a role from a *custom* policy, you must add `ssm.amazonaws.com` as a trusted entity to your role \(after you create it\)\. You add trusted entities on the **Trust Relationship** tab when viewing the role\. For example, you must add the following JSON block to the policy as a trusted entity\. For information about how to update a role to include a trusted entity, see [Modifying a Role](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html)\.   

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

For information about how to attach the role you just created to a new instance or an existing instance, see the next task\.

**Note**  
The access granted by the **AmazonEC2RoleforSSM** is very permissive, including read and write access to **all** of your S3 buckets\.  The minimum set of permissions required for Systems Manager to function are:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "",
            "Effect": "Allow",
            "Action": [
                "ssm:DescribeAssociation",
                "ec2messages:GetEndpoint",
                "ec2messages:AcknowledgeMessage",
                "ec2messages:GetMessages",
                "ssm:PutConfigurePackageResult",
                "ssm:ListInstanceAssociations",
                "ssm:UpdateAssociationStatus",
                "ec2messages:DeleteMessage",
                "ssm:UpdateInstanceInformation",
                "ec2messages:FailMessage",
                "ssm:GetDocument",
                "ec2messages:SendReply",
                "ssm:ListAssociations",
                "ssm:UpdateInstanceAssociationStatus"
            ],
            "Resource": "*"
        }
    ]
}
```

## Task 3: Create an Amazon EC2 Instance that Uses the Systems Manager Role<a name="sysman-create-instance-with-role"></a>

This procedure describes how to launch an Amazon EC2 instance that uses the role you just created\. You can also attach the role to an existing instance\. For more information, see [Attaching an IAM Role to an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide*\.

**To create an instance that uses the Systems Manager instance role**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Select a supported [region](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region)\.

1. Choose **Launch Instance** and select an instance\.

1. Choose your instance type and then choose **Next: Configure Instance Details**\.

1. In the **IAM role** drop\-down list choose the EC2 instance role you created earlier\.

1. Complete the wizard\.

If you create other instances that you want to configure using Systems Manager, you must specify the instance profile role for each instance\.

## Optional Access Configurations<a name="sysman-create-iam"></a>

Task 1 in this section enabled you to grant access to a user by choosing a pre\-existing or *managed* IAM user policy\. If you want to limit user access to Systems Manager and SSM documents, you can create your own restrictive user policies, as described in this section\. For more information about how to create a custom policy, see [Creating a New Policy](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html)\.

The following example IAM policy allows a user to do the following\.

+ List Systems Manager documents and document versions\.

+ View details about documents\.

+ Send a command using the document specified in the policy\.

  The name of the document is determined by this entry:

  ```
  arn:aws:ssm:us-east-1:*:document/name_of_restrictive_document
  ```

+ Send a command to three instances\.

  The instances are determined by the following entries in the second `Resource` section:

  ```
  "arn:aws:ec2:us-east-1:*:instance/i-1234567890abcdef0",
  "arn:aws:ec2:us-east-1:*:instance/i-0598c7d356eba48d7",
  "arn:aws:ec2:us-east-1:*:instance/i-345678abcdef12345",
  ```

+ View details about a command after it has been sent\.

+ Start and stop Automation executions\.

+ Get information about Automation executions\.

If you want to give a user permission to use this document to send commands on any instance for which the user currently has access \(as determined by their AWS user account\), you could specify the following entry in the `Resource` section and remove the other instance entries\.

```
"arn:aws:ec2:us-east-1:*:instance/*"
```

Note that the `Resource` section includes an Amazon S3 ARN entry:

```
arn:aws:s3:::bucket_name
```

You can also format this entry as follows:

```
arn:aws:s3:::bucket_name/*

-or-

arn:aws:s3:::bucket_name/key_prefix_name
```

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Action":[
            "ssm:ListDocuments",
            "ssm:ListDocumentsVersions",
            "ssm:DescribeDocument",
            "ssm:GetDocument",
            "ssm:DescribeInstanceInformation",
            "ssm:DescribeDocumentParameters",
			"ssm:DescribeInstanceProperties"
         ],
         "Effect":"Allow",
         "Resource":"*"
      },
      {
         "Action":"ssm:SendCommand",
         "Effect":"Allow",
         "Resource": [
                     "arn:aws:ec2:us-east-1:*:instance/i-1234567890abcdef0",
                     "arn:aws:ec2:us-east-1:*:instance/i-0598c7d356eba48d7",
                     "arn:aws:ec2:us-east-1:*:instance/i-345678abcdef12345",
                     "arn:aws:s3:::bucket_name",
                     "arn:aws:ssm:us-east-1:*:document/name_of_restrictive_document"
         ]
      },
      {
         "Action":[
            "ssm:CancelCommand",
            "ssm:ListCommands",
            "ssm:ListCommandInvocations"
         ],
         "Effect":"Allow",
         "Resource":"*"
      },
      {
         "Action":"ec2:DescribeInstanceStatus",
         "Effect":"Allow",
         "Resource":"*"
      },
      {
         "Action":"ssm:StartAutomationExecution",
         "Effect":"Allow",
         "Resource":[
            "arn:aws:ssm:::automation-definition/"
         ]
      },
      {
         "Action":"ssm:DescribeAutomationExecutions ",
         "Effect":"Allow",
         "Resource":[
            "*"
         ]
      },
      {
         "Action":[
            "ssm:StopAutomationExecution",
            "ssm:GetAutomationExecution"
         ],
         "Effect":"Allow",
         "Resource":[
            "arn:aws:ssm:::automation-execution/"
         ]
      }
   ]
}
```