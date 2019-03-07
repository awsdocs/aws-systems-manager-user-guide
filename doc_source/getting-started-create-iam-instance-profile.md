# Create a Custom IAM Instance Profile for Session Manager<a name="getting-started-create-iam-instance-profile"></a>

You can create a custom IAM instance profile that provides permissions for only Session Manager actions on your instances\. You can also create a policy to provide the permissions needed for logs of session activity to be sent to Amazon S3 and CloudWatch Logs\.

After you create an instance profile, see [Attaching an IAM Role to an Instance](https://docs.aws.amazon.com/IAM/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) and [Attach or Replace an Instance Profile](https://aws.amazon.com/premiumsupport/knowledge-center/attach-replace-ec2-instance-profile/) for information about how to attach the instance profile to an instance, For more information about IAM instance profiles and roles, see [Using Instance Profile](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) and [IAM Roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html) in the *IAM User Guide*\.

**Topics**
+ [Creating an Instance Profile with Minimal Session Manager Permissions \(Console\)](#create-iam-instance-profile-ssn-only)
+ [Creating an Instance Profile with Permissions for Session Manager and Amazon S3 and CloudWatch Logs \(Console\)](#create-iam-instance-profile-ssn-logging)

## Creating an Instance Profile with Minimal Session Manager Permissions \(Console\)<a name="create-iam-instance-profile-ssn-only"></a>

Use the following procedure to create a custom IAM instance profile with a policy that provides permissions for only Session Manager actions on your instances\.

**To create an instance profile with minimal Session Manager permissions \(console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\. \(If a **Get Started** button appears, choose it, and then choose **Create Policy**\.\)

1. Choose the **JSON** tab\.

1. Replace the default content with the following:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "ssm:UpdateInstanceInformation",
                   "ssmmessages:CreateControlChannel",
                   "ssmmessages:CreateDataChannel",
                   "ssmmessages:OpenControlChannel",
                   "ssmmessages:OpenDataChannel"
               ],
               "Resource": "*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "s3:GetEncryptionConfiguration"
               ],
               "Resource": "*"
           }
       ]
   }
   ```
**Note**  
For information about `ssmmessages`, see [Reference: ec2messages, ssmmessages, and Other API Calls](systems-manager-setting-up-messageAPIs.md)\.

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy\. For example: **SessionManagerPermissions**\.

1.  \(Optional\) For **Description**, enter a description for the policy\. 

1. Choose **Create policy**\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Create role** page, choose **AWS service**, and from the **Choose the service that will use this role** list, choose **EC2**\.

1. Choose **Next: Permissions**\.

1.  On the **Attached permissions policy** page, select the check box to the left of name of the policy you just created\. For example: **SessionManagerPermissions**\.

1. Choose **Next: Review**\.

1. On the **Review** page, for **Role name**, enter a name for the IAM instance profile\. For example **MySessionManagerInstanceProfile**\.

1. \(Optional\) For **Role description**, enter a description for the instance profile\. 

1. Choose **Create role**\.

## Creating an Instance Profile with Permissions for Session Manager and Amazon S3 and CloudWatch Logs \(Console\)<a name="create-iam-instance-profile-ssn-logging"></a>

Use the following procedure to create a custom IAM instance profile with a policy that provides permissions for Session Manager actions on your instances\. The policy also provides the permissions needed for session logs to be stored in Amazon S3 buckets and CloudWatch Logs log groups\.

For information about specifying preferences for storing session logs, see [Auditing and Logging Session Activity](session-manager-logging-auditing.md)\.

**To create an instance profile with permissions for Session Manager and Amazon S3 and CloudWatch Logs \(console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\. \(If a **Get Started** button appears, choose it, and then choose **Create Policy**\.\)

1. Choose the **JSON** tab\.

1. Replace the default content with the following\. Be sure to replace *s3\-bucket\-name* and *s3\-bucket\-prefix* with the names for your bucket and its prefix \(if any\)\. For information about `ssmmessages` in the following policy, see [Reference: ec2messages, ssmmessages, and Other API Calls](systems-manager-setting-up-messageAPIs.md)\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "ssmmessages:CreateControlChannel",
                   "ssmmessages:CreateDataChannel",
                   "ssmmessages:OpenControlChannel",
                   "ssmmessages:OpenDataChannel",
                   "ssm:UpdateInstanceInformation"
               ],
               "Resource": "*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogStream",
                   "logs:PutLogEvents",
                   "logs:DescribeLogGroups",
                   "logs:DescribeLogStreams"
               ],
               "Resource": "*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "s3:PutObject"
               ],
               "Resource": "arn:aws:s3:::s3-bucket-name/s3-bucket-prefix"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "s3:GetEncryptionConfiguration"
               ],
               "Resource": "*"
           },
           {
               "Effect": "Allow",
               "Action": "kms:GenerateDataKey",
               "Resource": "*"
           }
       ]
   }
   ```
**Important**  
To output session logs to an Amazon S3 bucket owned by a different AWS account, you must add the IAM `s3:PutObjectAcl` permission to this policy\. If this permission isn't added, the account that owns the S3 bucket cannot access the session output logs\.

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy\. For example: **SessionManagerPermissions**\.

1.  \(Optional\) For **Description**, enter a description for the policy\. 

1. Choose **Create policy**\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Create role** page, choose **AWS service**, and from the **Choose the service that will use this role** list, choose **EC2**\.

1. Choose **Next: Permissions**\.

1.  On the **Attached permissions policy** page, select the check box to the left of name of the policy you just created\. For example: **SessionManagerPermissions**\.

1. Choose **Next: Review**\.

1. On the **Review** page, for **Role name**, enter a name for the IAM instance profile\. For example **MySessionManagerInstanceProfile**\.

1. \(Optional\) For **Role description**, enter a description for the instance profile\. 

1. Choose **Create role**\.