# Step 1: Configure instance permissions for Systems Manager<a name="setup-instance-permissions"></a>

By default, AWS Systems Manager doesn't have permission to perform actions on your instances\. You can provide instance permissions at the account level using an AWS Identity and Access Management \(IAM\) role, or at the instance level using an instance profile\. If your use case allows, we recommend granting access at the account level using the Default Host Management Configuration\.

## Recommended configuration<a name="default-host-management"></a>

The Default Host Management Configuration allows Systems Manager to manage your Amazon EC2 instances automatically\. After you've turned on this setting, all instances using Instance Metadata Service Version 2 \(IMDSv2\) in your account with SSM Agent version 3\.2\.582\.0 or later installed automatically become managed instances\. Default Host Management Configuration doesn't support Instance Metadata Service Version 1\. For information about transitioning to IMDSv2, see [Transition to using Instance Metadata Service Version 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html#instance-metadata-transition-to-version-2) in the *Amazon EC2 User Guide for Linux Instances*\. For information about checking the version of the SSM Agent installed on your instance, see [Checking the SSM Agent version number](ssm-agent-get-version.md)\. For information about updating the SSM Agent, see [Automatically updating SSM Agent](ssm-agent-automatic-updates.md#ssm-agent-automatic-updates-console)\.

Benefits of managed instances include the following:
+ Connect to your instances securely using Session Manager\.
+ Perform automated patch scans using Patch Manager\.
+ View detailed information about your instances using Systems Manager Inventory\.
+ Track and manage instances using Fleet Manager\.
+ Keep the SSM Agent up to date automatically\.

Fleet Manager, Inventory, Patch Manager, and Session Manager are capabilities of AWS Systems Manager\.

Default Host Management Configuration allows instance management without the use of instance profiles and ensures that Systems Manager manages all instances in your account\. This allows you to separate your systems management permissions from application or other workload permissions you might need to provide to your instances\. You can also add policies to the default IAM role created by the Default Host Management Configuration if the permissions provided aren't sufficient for your use case\. If an instance already has an instance profile attached that contains permissions for Systems Manager, the SSM Agent attempts to use it before using the Default Host Management Configuration permissions\. For more information about the Default Host Management Configuration, see [Default Host Management Configuration](managed-instances-default-host-management.md)\.

**To turn on the Default Host Management Configuration setting**  
You can turn on the Default Host Management Configuration from the Fleet Manager console\. This procedure is intended to be performed by administrators\. To successfully complete this procedure using either the AWS Management Console or your preferred command line tool, you must have permissions for the `iam:PassRole` API operation for all resources, or for the `AmazonSSMManagedEC2InstanceDefaultRole` IAM role\.

**Note**  
You must turn on the Default Host Management Configuration in each AWS Region you wish to automatically manage your Amazon EC2 instances\. 

Before you begin, if you have instance profiles attached to your Amazon EC2 instances, remove any permissions that allow the `ssm:UpdateInstanceInformation` operation\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose **Default Host Management Configuration** under the **Account management** dropdown\.

1. Turn on **Enable Default Host Management Configuration**\.

1. Choose the IAM role used to enable Systems Manager capabilities for your instances\. We recommend using the default role provided by Default Host Management Configuration\. It contains the minimum set of permissions necessary to manage your Amazon EC2 instances using Systems Manager\. If you prefer to use a custom role, the role's trust policy must allow Systems Manager as a trusted entity\.

1. Choose **Configure** to complete setup\. 

After turning on the Default Host Management Configuration, it might take up 30 minutes for your instances to use the credentials of the role you chose\.

## Alternative configuration<a name="instance-profile-add-permissions"></a>

You can grant access at the individual instance level by using an AWS Identity and Access Management \(IAM\) instance profile\. An instance profile is a container that passes IAM role information to an Amazon Elastic Compute Cloud \(Amazon EC2\) instance at launch\. You can create an instance profile for Systems Manager by attaching one or more IAM policies that define the necessary permissions to a new role or to a role you already created\.

**Note**  
You can use Quick Setup, a capability of AWS Systems Manager, to quickly configure an instance profile on all instances in your AWS account\. Quick Setup also creates an IAM service role \(or *assume* role\), which allows Systems Manager to securely run commands on your instances on your behalf\. By using Quick Setup, you can skip this step \(Step 3\) and Step 4\. For more information, see [AWS Systems Manager Quick Setup](systems-manager-quick-setup.md)\. 

Note the following details about creating an IAM instance profile:
+ If you're configuring servers or virtual machines \(VMs\) in a hybrid environment for Systems Manager, you don't need to create an instance profile for them\. Instead, configure your servers and VMs to use an IAM service role\. For more information, see [Create an IAM service role for a hybrid environment](sysman-service-role.md)\.
+ If you change the IAM instance profile, it might take some time for the instance credentials to refresh\. SSM Agent won't process requests until this happens\. To speed up the refresh process, you can restart SSM Agent or restart the instance\.

Depending on whether you're creating a new role for your instance profile or adding the necessary permissions to an existing role, use one of the following procedures\.<a name="setup-instance-profile-managed-policy"></a>

**To create an instance profile for Systems Manager managed instances \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. For **Trusted entity type**, choose **AWS service**\.

1. Immediately under **Use case**, choose **EC2**, and then choose **Next**\.

1. On the **Add permissions** page, do the following: 
   + Use the **Search** field to locate the **AmazonSSMManagedInstanceCore** policy\. Select the check box next to its name\.   
![\[Choosing the EC2 service in the IAM console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/setup-instance-profile-2.png)

     The console retains your selection even if you search for other policies\.
   + If you created a custom S3 bucket policy in the previous procedure, [\(Optional\) Create a custom policy for S3 bucket access](#instance-profile-custom-s3-policy), search for it and select the check box next to its name\.
   + If you plan to join instances to an Active Directory managed by AWS Directory Service, search for **AmazonSSMDirectoryServiceAccess** and select the check box next to its name\.
   + If you plan to use EventBridge or CloudWatch Logs to manage or monitor your instance, search for **CloudWatchAgentServerPolicy** and select the check box next to its name\.

1. Choose **Next**\.

1. For **Role name**, enter a name for your new instance profile, such as **SSMInstanceProfile**\.
**Note**  
Make a note of the role name\. You will choose this role when you create new instances that you want to manage by using Systems Manager\.

1. \(Optional\) For **Description**, update the description for this instance profile\.

1. \(Optional\) For **Tags**, add one or more tag\-key value pairs to organize, track, or control access for this role, and then choose **Create role**\. The system returns you to the **Roles** page\.<a name="setup-instance-profile-custom-policy"></a>

**To add instance profile permissions for Systems Manager to an existing role \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose the existing role you want to associate with an instance profile for Systems Manager operations\.

1. On the **Permissions** tab, choose **Add permissions, Attach policies**\.

1. On the **Attach policy** page, do the following:
   + Use the **Search** field to locate the **AmazonSSMManagedInstanceCore** policy\. Select the check box next to its name\. 
   + If you have created a custom S3 bucket policy, search for it and select the check box next to its name\. For information about custom S3 bucket policies for an instance profile, see [\(Optional\) Create a custom policy for S3 bucket access](#instance-profile-custom-s3-policy)\.
   + If you plan to join instances to an Active Directory managed by AWS Directory Service, search for **AmazonSSMDirectoryServiceAccess** and select the check box next to its name\.
   + If you plan to use EventBridge or CloudWatch Logs to manage or monitor your instance, search for **CloudWatchAgentServerPolicy** and select the check box next to its name\.

1. Choose **Attach policies**\.

For information about how to update a role to include a trusted entity or further restrict access, see [Modifying a role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html) in the *IAM User Guide*\. 

## \(Optional\) Create a custom policy for S3 bucket access<a name="instance-profile-custom-s3-policy"></a>

Creating a custom policy for Amazon S3 access is required only if you're using a VPC endpoint or using an S3 bucket of your own in your Systems Manager operations\. You can attach this policy to the default IAM role created by the Default Host Management Configuration, or an instance profile you created in the previous procedure\.

For information about the AWS managed S3 buckets you provide access to in the following policy, see [SSM Agent communications with AWS managed S3 buckets](ssm-agent-minimum-s3-permissions.md)\.

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\. 

1. Choose the **JSON** tab, and replace the default text with the following\.

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

   **1** The first `Statement` element is required only if you're using a VPC endpoint\.

   **2** The second `Statement` element is required only if you're using an S3 bucket that you created to use in your Systems Manager operations\.

   **3** The `PutObjectAcl` access control list permission is required only if you plan to support cross\-account access to S3 buckets in other accounts\.

   **4** The `GetEncryptionConfiguration` element is required if your S3 bucket is configured to use encryption\.

   **5** If your S3 bucket is configured to use encryption, then the S3 bucket root \(for example, `arn:aws:s3:::DOC-EXAMPLE-BUCKET`\) must be listed in the **Resource** section\. Your user, group, or role must be configured with access to the root bucket\.

1. If you're using a VPC endpoint in your operations, do the following: 

   In the first `Statement` element, replace each *region* placeholder with the identifier of the AWS Region this policy will be used in\. For example, use `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.
**Important**  
We recommend that you avoid using wildcard characters \(\*\) in place of specific Regions in this policy\. For example, use `arn:aws:s3:::aws-ssm-us-east-2/*` and do not use `arn:aws:s3:::aws-ssm-*/*`\. Using wildcards could provide access to S3 buckets that you don’t intend to grant access to\. If you want to use the instance profile for more than one Region, we recommend repeating the first `Statement` element for each Region\.

   \-or\-

   If you aren't using a VPC endpoint in your operations, you can delete the first `Statement` element\.

1. If you're using an S3 bucket of your own in your Systems Manager operations, do the following:

   In the second `Statement` element, replace **DOC\-EXAMPLE\-BUCKET** with the name of an S3 bucket in your account\. You will use this bucket for your Systems Manager operations\. It provides permission for objects in the bucket, using `"arn:aws:s3:::my-bucket-name/*"` as the resource\. For more information about providing permissions for buckets or objects in buckets, see the topic [Amazon S3 actions](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html) in the *Amazon Simple Storage Service User Guide* and the AWS blog post [IAM Policies and Bucket Policies and ACLs\! Oh, My\! \(Controlling Access to S3 Resources\)](http://aws.amazon.com/blogs/security/iam-policies-and-bucket-policies-and-acls-oh-my-controlling-access-to-s3-resources/)\.
**Note**  
If you use more than one bucket, provide the ARN for each one\. See the following example for permissions on buckets\.  

   ```
   "Resource": [
   "arn:aws:s3:::DOC-EXAMPLE-BUCKET1/*",
   "arn:aws:s3:::DOC-EXAMPLE-BUCKET2/*"
                  ]
   ```

   \-or\-

   If you aren't using an S3 bucket of your own in your Systems Manager operations, you can delete the second `Statement` element\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add tags by choosing **Add tag**, and entering the preferred tags for the policy\.

1. Choose **Next: Review**\.

1. For **Name**, enter a name to identify this policy, such as **SSMInstanceProfileS3Policy**\.

1. Choose **Create policy**\.

## Additional policy considerations for managed instances<a name="instance-profile-policies-overview"></a>

This section describes some of the policies you can add to the default IAM role created by the Default Host Management Configuration, or your instance profiles for AWS Systems Manager\. To provide permissions for communication between instances and the Systems Manager API, we recommend creating custom policies that reflect your system needs and security requirements\. Depending on your operations plan, you might need permissions represented in one or more of the other policies\.

**Policy: `AmazonSSMDirectoryServiceAccess`**  
Required only if you plan to join Amazon EC2 instances for Windows Server to a Microsoft AD directory\.  
This AWS managed policy allows SSM Agent to access AWS Directory Service on your behalf for requests to join the domain by the managed instance\. For more information, see [Seamlessly join a Windows EC2 Instance](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/launching_instance.html) in the *AWS Directory Service Administration Guide*\.

**Policy: `CloudWatchAgentServerPolicy`**  
Required only if you plan to install and run the CloudWatch agent on your instances to read metric and log data on an instance and write it to Amazon CloudWatch\. These help you monitor, analyze, and quickly respond to issues or changes to your AWS resources\.  
Your default IAM role created by the Default Host Management Configuration or instance profile needs this policy only if you will use features such as Amazon EventBridge or Amazon CloudWatch Logs\. \(You can also create a more restrictive policy that, for example, limits writing access to a specific CloudWatch Logs log stream\.\)  
Using EventBridge and CloudWatch Logs features is optional\. However, we recommend setting them up at the beginning of your Systems Manager configuration process if you have decided to use them\. For more information, see the *[Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/)* and the *[Amazon CloudWatch Logs User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)*\.
To create IAM policies with permissions for additional Systems Manager capabilities, see the following resources:  
+ [Restricting access to Systems Manager parameters using IAM policies](sysman-paramstore-access.md)
+ [Setting up Automation](automation-setup.md)
+ [Verify or create an IAM role with Session Manager permissions](session-manager-getting-started-instance-profile.md)

## Attach the Systems Manager instance profile to an instance \(console\)<a name="attach-instance-profile"></a>

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Instances**, choose **Instances**\.

1. Navigate to and choose your EC2 instance from the list\.

1. In the **Actions** menu, choose **Security**, **Modify IAM role**\.

1. For **IAM role**, select the instance profile you created using the procedure in [Alternative configuration](#instance-profile-add-permissions)\.

1. Choose **Update **IAM role****\.

For more information about attaching IAM roles to instances, choose one of the following, depending on your selected operating system type:
+ [Attach an IAM role to an instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide for Linux Instances*
+ [Attach an IAM role to an instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide for Windows Instances*

Continue to [Step 2: Create VPC endpoints](setup-create-vpc.md)\.