# Add Session Manager Permissions to an Existing Instance Profile<a name="getting-started-add-permissions-to-existing-profile"></a>

Follow these steps to embed Session Manager permissions in an existing an IAM instance profile that does not rely on the AWS\-provided default policy **AmazonEC2RoleforSSM** for instance permissions\. Note that this procedure assumes that your existing profile already includes other Systems Manager `ssm` permissions for actions you want to allow access to\. This policy alone is not enough to use Session Manager\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. In the list, choose the name of the role to embed a policy in\.

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
           }
       ]
   }
   ```
**Note**  
For information about `ssmmessages`, see [Reference: ec2messages, ssmmessages, and Other API Calls](systems-manager-setting-up-messageAPIs.md)\.

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy\. For example: **SessionManagerPermissions**\.

1. Choose **Create policy**\.