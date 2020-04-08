# Adding Session Manager permissions to an existing instance profile<a name="getting-started-add-permissions-to-existing-profile"></a>

Follow these steps to embed Session Manager permissions in an existing IAM instance profile that does not rely on the AWS\-provided default policy **AmazonSSMManagedInstanceCore** for instance permissions\. Note that this procedure assumes that your existing profile already includes other Systems Manager `ssm` permissions for actions you want to allow access to\. This policy alone is not enough to use Session Manager\.

**To add Session Manager permissions to an existing instance profile \(console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. Choose the name of the role to embed a policy in\.

1. Choose the **Permissions** tab\.

1. Scroll to the bottom of the page and choose **Add inline policy**\. 

1. Choose the **JSON** tab\.

1. Replace the default content with the following:

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

1. Choose **Create policy**\.