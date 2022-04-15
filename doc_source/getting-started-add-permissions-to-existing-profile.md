# Adding Session Manager permissions to an existing IAM role<a name="getting-started-add-permissions-to-existing-profile"></a>

Follow these steps to embed Session Manager permissions in an existing AWS Identity and Access Management \(IAM\) role that doesn't rely on the AWS\-provided default policy `AmazonSSMManagedInstanceCore` for instance permissions\. This procedure assumes that your existing role already includes other Systems Manager `ssm` permissions for actions you want to allow access to\. This policy alone isn't enough to use Session Manager\.

**To add Session Manager permissions to an existing role \(console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. Choose the name of the role to embed a policy in\.

1. Choose the **Permissions** tab\.

1. Choose **Add inline policy**\. The link is located on the right side of the page\.

1. Choose the **JSON** tab\.

1. Replace the default content with the following policy\. Replace *key\-name* with the Amazon Resource Name \(ARN\) of the KMS key you want to use\.

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

   Choose **Create policy**\.

For information about the `ssmmessages` actions, see [Reference: ec2messages, ssmmessages, and other API operations](systems-manager-setting-up-messageAPIs.md)\.