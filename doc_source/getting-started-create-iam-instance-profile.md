# Create a custom IAM instance profile for Session Manager<a name="getting-started-create-iam-instance-profile"></a>

You can create a custom IAM instance profile that provides permissions for only Session Manager actions on your instances\. You can also create a policy to provide the permissions needed for logs of session activity to be sent to Amazon S3 and CloudWatch Logs\.

After you create an instance profile, see [Attaching an IAM Role to an Instance](https://docs.aws.amazon.com/IAM/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) and [Attach or Replace an Instance Profile](https://aws.amazon.com/premiumsupport/knowledge-center/attach-replace-ec2-instance-profile/) for information about how to attach the instance profile to an instance, For more information about IAM instance profiles and roles, see [Using Instance Profile](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) and [IAM roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html) in the *IAM User Guide*\.

**Topics**
+ [Creating an instance profile with minimal Session Manager permissions \(console\)](#create-iam-instance-profile-ssn-only)
+ [Creating an instance profile with permissions for Session Manager and Amazon S3 and CloudWatch Logs \(console\)](#create-iam-instance-profile-ssn-logging)

## Creating an instance profile with minimal Session Manager permissions \(console\)<a name="create-iam-instance-profile-ssn-only"></a>

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
           },
           {
               "Effect": "Allow",
               "Action": [
                   "kms:Decrypt"
               ],
               "Resource": "key-name"
           }
       ]
   }
   ```

**About 'ssmmessages'**  
For information about `ssmmessages`, see [Reference: ec2messages, ssmmessages, and other API calls](systems-manager-setting-up-messageAPIs.md)\.

**About 'kms:Decrypt'**  
In this policy, the `kms:Decrypt` permission enables customer key encryption and decryption for session data\. If you will use AWS Key Management Service \(AWS KMS\) encryption for your session data, replace *key\-name* with the ARN of the customer master key \(CMK\) you want to use, in the format `arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-12345EXAMPLE`\. 

   If you will not use AWS KMS encryption for your session data, you can remove the following content from the policy:

   ```
   ,
           {
               "Effect": "Allow",
               "Action": [
                   "kms:Decrypt"
               ],
               "Resource": "key-name"
           }
   ```

   For information about using AWS KMS and a CMK to encrypt session data, see [Enable AWS KMS key encryption of session data \(console\)](session-preferences-enable-encryption.md)\.

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy, such as **SessionManagerPermissions**\.

1. \(Optional\) For **Description**, enter a description for the policy\. 

1. Choose **Create policy**\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Create role** page, choose **AWS service**, and from the **Choose the service that will use this role** list, choose **EC2**\.

1. Choose **Next: Permissions**\.

1. On the **Attached permissions policy** page, select the check box to the left of name of the policy you just created, such as **SessionManagerPermissions**\.

1. Choose **Next: Review**\.

1. On the **Review** page, for **Role name**, enter a name for the IAM instance profile, such as **MySessionManagerInstanceProfile**\.

1. \(Optional\) For **Role description**, enter a description for the instance profile\. 

1. Choose **Create role**\.

## Creating an instance profile with permissions for Session Manager and Amazon S3 and CloudWatch Logs \(console\)<a name="create-iam-instance-profile-ssn-logging"></a>

Use the following procedure to create a custom IAM instance profile with a policy that provides permissions for Session Manager actions on your instances\. The policy also provides the permissions needed for session logs to be stored in S3 buckets and CloudWatch Logs log groups\.

For information about specifying preferences for storing session logs, see [Auditing and logging session activity](session-manager-logging-auditing.md)\.

**To create an instance profile with permissions for Session Manager and Amazon S3 and CloudWatch Logs \(console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\. \(If a **Get Started** button appears, choose it, and then choose **Create Policy**\.\)

1. Choose the **JSON** tab\.

1. Replace the default content with the following\. Be sure to replace *DOC\-EXAMPLE\-BUCKET* and *s3\-bucket\-prefix* with the names for your bucket and its prefix \(if any\)\. For information about `ssmmessages` in the following policy, see [Reference: ec2messages, ssmmessages, and other API calls](systems-manager-setting-up-messageAPIs.md)\.

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
               "Resource": "arn:aws:s3:::DOC-EXAMPLE-BUCKET/s3-bucket-prefix"
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
To output session logs to an S3 bucket owned by a different AWS account, you must add the IAM `s3:PutObjectAcl` permission to this policy\. If this permission isn't added, the account that owns the S3 bucket cannot access the session output logs\.

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy, such as **SessionManagerPermissions**\.

1. \(Optional\) For **Description**, enter a description for the policy\. 

1. Choose **Create policy**\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Create role** page, choose **AWS service**, and from the **Choose the service that will use this role** list, choose **EC2**\.

1. Choose **Next: Permissions**\.

1. On the **Attached permissions policy** page, select the check box to the left of name of the policy you just created, such as **SessionManagerPermissions**\.

1. Choose **Next: Review**\.

1. On the **Review** page, for **Role name**, enter a name for the IAM instance profile, such as **MySessionManagerInstanceProfile**\.

1. \(Optional\) For **Role description**, enter a description for the instance profile\. 

1. Choose **Create role**\.