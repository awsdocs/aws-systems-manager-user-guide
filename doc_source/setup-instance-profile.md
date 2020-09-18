# Step 4: Create an IAM instance profile for Systems Manager<a name="setup-instance-profile"></a>

By default, AWS Systems Manager doesn't have permission to perform actions on your instances\. You must grant access by using an AWS Identity and Access Management \(IAM\) instance profile\. An instance profile is a container that passes IAM role information to an Amazon Elastic Compute Cloud \(Amazon EC2\) instance at launch\. You can create an instance profile for Systems Manager by attaching one or more IAM policies that define the necessary permissions to a new role or to a role you already created\.

**Note**  
You can use Systems Manager Quick Setup to quickly configure an instance profile on all instances in your AWS account\. Quick Setup also creates an IAM service role \(or *assume* role\), which enables Systems Manager to securely run commands on your instances on your behalf\. By using Quick Setup, you can skip this step \(Step 4\) and Step 5\. For more information, see [AWS Systems Manager Quick Setup](systems-manager-quick-setup.md)\. 

Note the following details about creating an IAM instance profile:
+ If you are configuring servers or virtual machines \(VMs\) in a hybrid environment for Systems Manager, you don't need to create an instance profile for them\. Instead, you must configure your servers and VMs to use an IAM service role\. For more information, see [Create an IAM service role for a hybrid environment](sysman-service-role.md)\.
+ If you change the IAM instance profile, it might take some time for the instance credentials to refresh\. SSM Agent will not process requests until this happens\. To speed up the refresh process, you can restart SSM Agent or restart the instance\.

## About policies for a Systems Manager instance profile<a name="instance-profile-policies-overview"></a>

This section describes the policies you can add to your EC2 instance profile for Systems Manager\. To provide permissions for communication between instances and the Systems Manager API, we recommend creating custom policies that take into account your system needs and security requirements\. However, as a starting point, you can use one or more of the following policies to grant permission for Systems Manager to interact with your instances\. The first policy, **AmazonSSMManagedInstanceCore**, enables an instance to use AWS Systems Manager service core functionality\. Depending on your operations plan, you might need permissions represented in one or more of the other three policies\.

**Policy: AmazonSSMManagedInstanceCore**  
Required permissions\.  
This AWS managed policy enables an instance to use Systems Manager service core functionality\.

**Policy: A custom policy for S3 bucket access**  
Required permissions in either of the following cases:  
+ **Case 1**: You are using a VPC endpoint to privately connect your VPC to supported AWS services and VPC endpoint services powered by PrivateLink\. 

  SSM Agent is Amazon software that is installed on your instances and performs Systems Manager tasks\. This agent requires access to specific Amazon\-owned S3 buckets\. These buckets are publicly accessible\. 

  In a private VPC endpoint environment, however, you must explicitly provide access to these buckets:

  ```
  arn:aws:s3:::patch-baseline-snapshot-region/*
  arn:aws:s3:::aws-ssm-region/*
  ```

  For more information, see [Step 6: \(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md), [About minimum S3 Bucket permissions for SSM Agent](ssm-agent-minimum-s3-permissions.md), and [VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html) in the *Amazon VPC User Guide*\.
+ **Case 2**: You plan to use an S3 bucket that you create as part of your Systems Manager operations\.

  Your EC2 instance profile for Systems Manager must grant access to an S3 bucket that you own for tasks like the following: 
  + To access scripts you store in the S3 bucket to use in commands you run\.
  + To store the full output of Run Command commands or Session Manager sessions\.
  + To access custom patch lists for use when patching your instances\.
Saving output log data in an S3 bucket is optional, but we recommend setting it up at the beginning of your Systems Manager configuration process if you have decided to do so\. For more information, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\.

**Policy: AmazonSSMDirectoryServiceAccess**  
Required only if you plan to join EC2 instance for Windows Server to a Microsoft AD directory\.  
This AWS managed policy allows SSM Agent to access AWS Directory Service on your behalf for requests to join the domain by the managed instance\. For more information, see [Seamlessly Join a Windows EC2 Instance](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/launching_instance.html) in the *AWS Directory Service Administration Guide*\.

**Policy: CloudWatchAgentServerPolicy**  
Required only if you plan to install and run the CloudWatch agent on your instances to read metric and log data on an instance and write it to Amazon CloudWatch\. These help you monitor, analyze, and quickly respond to issues or changes to your AWS resources\.  
Your instance profile needs this policy only if you will use features such as Amazon EventBridge or CloudWatch Logs\. \(You can also create a more restrictive policy that, for example, limits writing access to a specific CloudWatch Logs log stream\.\)  
Using EventBridge and CloudWatch Logs features is optional, but we recommend setting them up at the beginning of your Systems Manager configuration process if you have decided to use them\. For more information, see the *[Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/)* and the *[Amazon CloudWatch Logs User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)*\.
To create an instance profile with permissions for additional Systems Manager services, see the following resources\.  
+ [Restricting access to Systems Manager parameters using IAM policies](sysman-paramstore-access.md)
+ [Getting started with Automation](automation-setup.md)
+ [Verify or create an IAM instance profile with Session Manager permissions](session-manager-getting-started-instance-profile.md)
+ [Setting up Run Command](sysman-rc-setting-up.md)

## Task 1: \(Optional\) Create a custom policy for S3 bucket access<a name="instance-profile-custom-s3-policy"></a>

Creating a custom policy for Amazon S3 access is required only if you are using a VPC endpoint or using an S3 bucket of your own in your Systems Manager operations\.

For information about the AWS managed S3 buckets you provide access to in the policy below, see [About minimum S3 Bucket permissions for SSM Agent](ssm-agent-minimum-s3-permissions.md)\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\. 

1. Choose the **JSON** tab, and replace the default text with the following:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout01.png){
               "Effect": "Allow",
               "Action": "s3:GetObject",
               "Resource": [
                   "arn:aws:s3:::aws-ssm-region/*",
                   "arn:aws:s3:::aws-windows-downloads-region/*",
                   "arn:aws:s3:::amazon-ssm-region/*",
                   "arn:aws:s3:::amazon-ssm-packages-region/*",
                   "arn:aws:s3:::region-birdwatcher-prod/*",
                   "arn:aws:s3:::aws-ssm-distributor-file-region/*",
                   "arn:aws:s3:::aws-ssm-document-attachments-region/*",
                   "arn:aws:s3:::patch-baseline-snapshot-region/*"
               ]
           },
           ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout02.png){
               "Effect": "Allow",
               "Action": [
                   "s3:GetObject",
                   "s3:PutObject",
                   "s3:PutObjectAcl", ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout03.png)
                   "s3:GetEncryptionConfiguration" ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout04.png)
               ],
               "Resource": [
                   "arn:aws:s3:::DOC-EXAMPLE-BUCKET/*",
                   "arn:aws:s3:::DOC-EXAMPLE-BUCKET" ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout05.png)
               ]
           }
       ]
   }
   ```

   **1** The first `Statement` element is required only if you are using a VPC endpoint\.

   **2** The second `Statement` element is required only if you are using an S3 bucket that you created to use in your Systems Manager operations\.

   **3** The `PutObjectAcl` access control list permission is required only if you plan to support cross\-account access to S3 buckets in other accounts\.

   **4** The `GetEncryptionConfiguration` element is required if your S3 bucket is configured to use encryption\.

   **5** If your S3 bucket is configured to use encryption, then the S3 bucket root \(for example, arn:aws:s3:::*DOC\-EXAMPLE\-BUCKET*\) must be listed in the **Resource** section\. Your IAM user, group, or role must be configured with access to the root bucket\.

1. If you are using a VPC endpoint in your operations, do the following: 

   In the first `Statement` element, replace each *region* placeholder with the identifier of the AWS Region this policy will be used in\. For example, use `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.
**Important**  
We recommend that you avoid using wildcard characters \(\*\) in place of specific Regions in this policy\. For example, use `arn:aws:s3:::aws-ssm-us-east-2/*` and do not use `arn:aws:s3:::aws-ssm-*/*`\. Using wildcards could provide access to S3 buckets that you don’t intend to grant access to\. If you want to use the instance profile for more than one Region, we recommend repeating the first `Statement` element for each Region\.

   \-or\-

   If you are not using a VPC endpoint in your operations, you can delete the first `Statement` element\.

1. If you are using an S3 bucket of your own in your Systems Manager operations, do the following:

   In the second `Statement` element, replace **DOC\-EXAMPLE\-BUCKET** with the name of an S3 bucket in your account\. You will use this bucket for your Systems Manager operations\. It provides permission for objects in the bucket, using `"arn:aws:s3:::my-bucket-name/*"` as the resource\. For more information about providing permissions for buckets or objects in buckets, see the topic [Specifying Permissions in a Policy](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html) in the *Amazon Simple Storage Service Developer Guide* and the AWS blog post [IAM Policies and Bucket Policies and ACLs\! Oh, My\! \(Controlling Access to S3 Resources\)](http://aws.amazon.com/blogs/security/iam-policies-and-bucket-policies-and-acls-oh-my-controlling-access-to-s3-resources/)\.
**Note**  
If you use more than one bucket, provide the ARN for each one\. For example, for permissions on buckets:  

   ```
   "Resource": [
   "arn:aws:s3:::DOC-EXAMPLE-BUCKET1/*",
   "arn:aws:s3:::DOC-EXAMPLE-BUCKET2/*"
                  ]
   ```

   \-or\-

   If you are not using an S3 bucket of your own in your Systems Manager operations, you can delete the second `Statement` element\.

1. Choose **Review policy**\.

1. For **Name**, enter a name to identify this policy, such as **SSMInstanceProfileS3Policy** or another name that you prefer\.

1. Choose **Create policy**\.

## Task 2: Add permissions to a Systems Manager instance profile \(console\)<a name="instance-profile-add-permissions"></a>

Depending on whether you are creating a new role for your instance profile or adding the necessary permissions to an existing role, use one of the following procedures\.<a name="setup-instance-profile-managed-policy"></a>

**To create an instance profile for Systems Manager managed instances \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. Under **Select type of trusted entity**, choose **AWS service**\.

1. Immediately under **Choose the service that will use this role**, choose **EC2**, and then choose **Next: Permissions**\.  
![\[Choosing the EC2 service in the IAM console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/setup-instance-profile.png)

1. On the **Attach permissions policies** page, do the following: 
   + Use the **Search** field to locate the **AmazonSSMManagedInstanceCore**\. Select the box next to its name\.   
![\[Choosing the EC2 service in the IAM console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/setup-instance-profile-2.png)

     The console retains your selection even if you search for other policies\.
   + If you created a custom S3 bucket policy in the previous procedure, [Task 1: \(Optional\) Create a custom policy for S3 bucket access](#instance-profile-custom-s3-policy), search for it and select the box next to its name\.
   + If you plan to join instances to an Active Directory managed by AWS Directory Service, search for **AmazonSSMDirectoryServiceAccess** and select the box next to its name\.
   + If you plan to use EventBridge or CloudWatch Logs to manage or monitor your instance, search for **CloudWatchAgentServerPolicy** and select the box next to its name\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add one or more tag\-key value pairs to organize, track, or control access for this role, and then choose **Next: Review**\. 

1. For **Role name**, enter a name for your new instance profile, such as **SSMInstanceProfile** or another name that you prefer\.
**Note**  
Make a note of the role name\. You will choose this role when you create new instances that you want to manage by using Systems Manager\.

1. \(Optional\) For **Role description**, enter a description for this instance profile\.

1. Choose **Create role**\. The system returns you to the **Roles** page\.<a name="setup-instance-profile-custom-policy"></a>

**To add instance profile permissions for Systems Manager to an existing role \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose the existing role you want to associate with an instance profile for Systems Manager operations\.

1. On the **Permissions** tab, choose **Attach policies**\.

1. On the **Attach permission policies** page, do the following:
   + Select the box next to the required **AmazonSSMManagedInstanceCore** managed policy\.
   + If you have created a custom S3 bucket policy, select the box next to its name\. For information about custom S3 bucket policies for an instance profile, see [Task 1: \(Optional\) Create a custom policy for S3 bucket access](#instance-profile-custom-s3-policy)\.
   + If you plan to join instances to an Active Directory managed by AWS Directory Service, select the box next to **AmazonSSMDirectoryServiceAccess**\.
   + If you plan to use EventBridge or CloudWatch Logs to manage or monitor your instance, select the box next to **CloudWatchAgentServerPolicy**\.

1. Choose **Attach policy**\.

For information about how to update a role to include a trusted entity or further restrict access, see [Modifying a Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the *IAM User Guide*\. 

Continue to [Step 5: Attach an IAM instance profile to an EC2 instance](setup-launch-managed-instance.md)\.