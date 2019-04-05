# Enable AWS KMS Key Encryption of Session Data \(Console\)<a name="session-preferences-enable-encryption"></a>

AWS Key Management Service \(AWS KMS\) lets you create and manage keys and control the use of encryption across a wide range of AWS services and in your applications\. You can also specify that session data transmitted between your Amazon EC2 instances and the local machines of users in your AWS account is encrypted using AWS KMS key encryption\. \(This is in addition to the TLS 1\.2 encryption that AWS already provides by default\.\) AWS KMS key encryption for sessions is accomplished using a customer master key \(CMK\) that has been created in AWS KMS\. Both the user starting the Session Manager session and the instance that the session connects to must have permission to use the key\. For information about IAM policy permissions for using a CMK with Session Manager, see [Step 2: Verify or Create an IAM Instance Profile with Session Manager Permissions](session-manager-getting-started-instance-profile.md) and [Quickstart Default IAM Policies for Session Manager](getting-started-restrict-access-quickstart.md)\.

You can use a key that you have created in your AWS account\. You can also use a key that has been created in a different AWS account, provided that the creator of that key has provided you with the permissions needed to use the key\.

You can also use the CLI to specify or change the AWS KMS key that is used to encrypt session data\. For information, see [Update Session Manager Preferences \(AWS CLI\)](getting-started-configure-preferences-cli.md)\.

For information about creating and managing AWS KMS keys, see [https://docs.aws.amazon.com/kms/latest/developerguide/](https://docs.aws.amazon.com/kms/latest/developerguide/)\.

**Note**  
CMKs are priced resources\. For information, see [AWS Key Management Service pricing](docs.aws.amazon.comkms/pricing/)\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Select the check box next to **Key Management Service \(KMS\)**\.

1. Do one of the following:
   + Choose the button beside **Select an AWS KMS key in my current account**, then select a key from the list\.

     \-or\-

     Choose the button beside **Enter a KMS key alias or KMS key ARN**\. Manually enter an AWS KMS key alias for a key created in your current account, or enter the key ARN for a key in another account\. For example:
     + Key alias: `alias/my-kms-key-alias`
     + Key ARN: `arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-12345EXAMPLE`

     \-or\-

     Choose **Create new key** to create a new CMK in your account\. After you create the new key, return to the **Preferences** tab and select the key for encrypting session data in your account\.

   For more information about sharing keys, see [Allowing External AWS Accounts to Access a CMK](https://docs.aws.amazon.com/kms/latest/developerguide/key-policy-modifying.html#key-policy-modifying-external-accounts) in the *AWS Key Management Service Developer Guide*\.

1. Choose **Save**\.