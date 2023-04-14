# Turn on KMS key encryption of session data \(console\)<a name="session-preferences-enable-encryption"></a>

Use AWS Key Management Service \(AWS KMS\) to create and manage keys\. With AWS KMS, you can control the use of encryption across a wide range of AWS services and in your applications\. You can specify that session data transmitted between your managed nodes and the local machines of users in your AWS account is encrypted using KMS key encryption\. \(This is in addition to the TLS 1\.2 encryption that AWS already provides by default\.\) KMS key encryption for sessions is accomplished using a key that is created in AWS KMS\. AWS KMS encryption is available for `Standard_Stream`, `InteractiveCommands`, and `NonInteractiveCommands` session types\. To use the option to encrypt session data using a key created in AWS KMS, version 2\.3\.539\.0 or later of AWS Systems Manager SSM Agent must be installed on the managed node\. 

**Note**  
You must allow AWS KMS encryption in order to reset passwords on your managed nodes from the AWS Systems Manager console\. For more information, see [Reset a password on a managed node](managed-instances-password-reset.md#managed-instance-reset-a-password)\.

You can use a key that you created in your AWS account\. You can also use a key that was created in a different AWS account\. The creator of the key in a different AWS account must provide you with the permissions needed to use the key\.

After you turn on KMS key encryption for your session data, both the users who start sessions and the managed nodes that they connect to must have permission to use the key\. You provide permission to use the KMS key with Session Manager through AWS Identity and Access Management \(IAM\) policies\. For information, see the following topics:
+ Add AWS KMS permissions for users in your account: [Quickstart default IAM policies for Session Manager](getting-started-restrict-access-quickstart.md)\.
+ Add AWS KMS permissions for managed nodes in your account: [Step 2: Verify or create an IAM role with Session Manager permissions](session-manager-getting-started-instance-profile.md)\.

For more information about creating and managing KMS keys, see the [https://docs.aws.amazon.com/kms/latest/developerguide/](https://docs.aws.amazon.com/kms/latest/developerguide/)\.

For information about using the AWS CLI to turn on KMS key encryption of session data in your account, see [Create a Session Manager preferences document \(command line\)](getting-started-create-preferences-cli.md) or [Update Session Manager preferences \(command line\)](getting-started-configure-preferences-cli.md)\.

**Note**  
There is a charge to use KMS keys\. For information, see [AWS Key Management Service pricing](http://aws.amazon.com/kms/pricing/)\.

**To turn on KMS key encryption of session data \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Select the check box next to **Enable KMS encryption**\.

1. Do one of the following:
   + Choose the button next to **Select a KMS key in my current account**, then select a key from the list\.

     \-or\-

     Choose the button next to **Enter a KMS key alias or KMS key ARN**\. Manually enter a KMS key alias for a key created in your current account, or enter the key Amazon Resource Name \(ARN\) for a key in another account\. The following are examples:
     + Key alias: `alias/my-kms-key-alias`
     + Key ARN: `arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-12345EXAMPLE`

     \-or\-

     Choose **Create new key** to create a new KMS key in your account\. After you create the new key, return to the **Preferences** tab and select the key for encrypting session data in your account\.

   For more information about sharing keys, see [Allowing External AWS accounts to Access a key](https://docs.aws.amazon.com/kms/latest/developerguide/key-policy-modifying.html#key-policy-modifying-external-accounts) in the *AWS Key Management Service Developer Guide*\.

1. Choose **Save**\.