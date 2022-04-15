# Create a custom IAM role for Session Manager<a name="getting-started-create-iam-instance-profile"></a>

You can create a custom AWS Identity and Access Management \(IAM\) role that provides permissions for only Session Manager actions on your instances\. You can also include a policy to provide the permissions needed for session logs to be sent to Amazon Simple Storage Service \(Amazon S3\) and Amazon CloudWatch Logs\.

After you create the IAM role, see [Attaching an IAM Role to an Instance](https://docs.aws.amazon.com/IAM/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) and [Attach or Replace an Instance Profile](https://aws.amazon.com/premiumsupport/knowledge-center/attach-replace-ec2-instance-profile/) for information about how to attach the role to an instance\. For more information about IAM instance profiles and roles, see [Using Instance Profile](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html) and [IAM roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html) in the *IAM User Guide*\. For more information about creating an IAM service role for on\-premises machines, see [Create an IAM service role for a hybrid environment](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-service-role.html)\.

**Topics**
+ [Creating an IAM role with minimal Session Manager permissions \(console\)](#create-iam-instance-profile-ssn-only)
+ [Creating an IAM role with permissions for Session Manager and Amazon S3 and CloudWatch Logs \(console\)](#create-iam-instance-profile-ssn-logging)

## Creating an IAM role with minimal Session Manager permissions \(console\)<a name="create-iam-instance-profile-ssn-only"></a>

Use the following procedure to create a custom IAM role with a policy that provides permissions for only Session Manager actions on your instances\.

**To create an instance profile with minimal Session Manager permissions \(console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\. \(If a **Get Started** button is displayed, choose it, and then choose **Create Policy**\.\)

1. Choose the **JSON** tab\.

1. Replace the default content with the following policy\. Replace *key\-name* with the Amazon Resource Name \(ARN\) of the KMS key you want to use\.

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

   For information about using a KMS key to encrypt session data, see [Turn on KMS key encryption of session data \(console\)](session-preferences-enable-encryption.md)\.

   If you won't use AWS KMS encryption for your session data, you can remove the following content from the policy\.

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

1. Choose **Next: Tags**\.

1. \(Optional\) Add tags by choosing **Add tag**, and entering the preferred tags for the policy\.

1. Choose **Next: Review**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy, such as **SessionManagerPermissions**\.

1. \(Optional\) For **Description**, enter a description for the policy\. 

1. Choose **Create policy**\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Create role** page, choose **AWS service**, and for **Use case**, choose **EC2**\.

1. Choose **Next**\.

1. On the **Add permissions** page, select the check box to the left of name of the policy you just created, such as **SessionManagerPermissions**\.

1. Choose **Next**\.

1. On the **Name, review, and create** page, for **Role name**, enter a name for the IAM role, such as **MySessionManagerRole**\.

1. \(Optional\) For **Role description**, enter a description for the instance profile\. 

1. \(Optional\) Add tags by choosing **Add tag**, and entering the preferred tags for the role\.

   Choose **Create role**\.

For information about `ssmmessages` actions, see [Reference: ec2messages, ssmmessages, and other API operations](systems-manager-setting-up-messageAPIs.md)\.

## Creating an IAM role with permissions for Session Manager and Amazon S3 and CloudWatch Logs \(console\)<a name="create-iam-instance-profile-ssn-logging"></a>

Use the following procedure to create a custom IAM role with a policy that provides permissions for Session Manager actions on your instances\. The policy also provides the permissions needed for session logs to be stored in Amazon Simple Storage Service \(Amazon S3\) buckets and Amazon CloudWatch Logs log groups\.

**Important**  
To output session logs to an Amazon S3 bucket owned by a different AWS account, you must add the IAM `s3:PutObjectAcl` permission to the policy\. If this permission isn't added, the account that owns the Amazon S3 bucket can't access the session output logs\.

For information about specifying preferences for storing session logs, see [Logging session activity](session-manager-logging.md)\.

**To create an IAM role with permissions for Session Manager and Amazon S3 and CloudWatch Logs \(console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\. \(If a **Get Started** button is displayed, choose it, and then choose **Create Policy**\.\)

1. Choose the **JSON** tab\.

1. Replace the default content with the following policy\. Replace each *example resource placeholder* with your own information\.

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
               "Resource": "arn:aws:s3:::DOC-EXAMPLE-BUCKET/s3-bucket-prefix/*"
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
           },
           {
               "Effect": "Allow",
               "Action": "kms:GenerateDataKey",
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Next: Tags**\.

1. \(Optional\) Add tags by choosing **Add tag**, and entering the preferred tags for the policy\.

1. Choose **Next: Review**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy, such as **SessionManagerPermissions**\.

1. \(Optional\) For **Description**, enter a description for the policy\. 

1. Choose **Create policy**\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Create role** page, choose **AWS service**, and for **Use case**, choose **EC2**\.

1. Choose **Next**\.

1. On the **Add permissions** page, select the check box to the left of name of the policy you just created, such as **SessionManagerPermissions**\.

1. Choose **Next**\.

1. On the **Name, review, and create** page, for **Role name**, enter a name for the IAM role, such as **MySessionManagerRole**\.

1. \(Optional\) For **Role description**, enter a description for the role\. 

1. \(Optional\) Add tags by choosing **Add tag**, and entering the preferred tags for the role\.

1. Choose **Create role**\.