# Enforce Document Permission Check for Default CLI Scenario<a name="getting-started-sessiondocumentaccesscheck"></a>

When you configure Session Manager for your account, the system creates an SSM document named `SSM-SessionManagerRunShell`\. This SSM document stores your requirements for whether session data is saved in an Amazon S3 bucket or Amazon CloudWatch Logs log group, whether session data is encrypted using AWS Key Management Service, and whether Run As support is enabled for your sessions\. The following is an example\.

```
{
  "schemaVersion": "1.0",
  "description": "Document to hold regional settings for Session Manager",
  "sessionType": "Standard_Stream",
  "inputs": {
    "s3BucketName": "MyBucketName",
    "s3KeyPrefix": "MyBucketPrefix",
    "s3EncryptionEnabled": true,
    "cloudWatchLogGroupName": "MyLogGroupName",
    "cloudWatchEncryptionEnabled": true,
    "kmsKeyId": "MyKMSKeyID",
    "runAsEnabled": true,
    "runAsDefaultUser": "MyDefaultRunAsUser"
  }
}
```

By default, if a user in your account has been granted permission in their IAM user policy to start sessions, that user has access to this SSM document\. This means that when they use the AWS CLI to run the `[start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html)` command, and they do not specify a configuration document, the system uses `SSM-SessionManagerRunShell` and launches the full interactive shell\. The session starts even if the user’s IAM policy doesn’t grant explicit permission to access the `SSM-SessionManagerRunShell` document\.

For example, the following command doesn’t specify a Session Manager configuration document\.

```
aws ssm start-session --target i-02573cafcfEXAMPLE
```

The following example does specify the default Session Manager configuration document\.

```
aws ssm start-session --document-name SSM-SessionManagerRunShell --target i-02573cafcfEXAMPLE
```

For cases where the user doesn’t specify a document name in the `[start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html)` CLI command, you can ensure that the user has been granted explicit access to this default configuration document\. You do this by adding the following condition element to the IAM policy that controls the user’s access to sessions: 

```
"Condition": {
                "BoolIfExists": {
                    "ssm:SessionDocumentAccessCheck": "true"
                }
            }
```

With this condition element set to true in the user’s associated IAM policy, explicit access to `SSM-SessionManagerRunShell` must be granted in the IAM policy\. The following is an example\. 

```
{
            "Effect": "Allow",
            "Action": [
                "ssm:StartSession"
            ],
            "Resource": "arn:aws:ssm:region:account-id:document/SSM-SessionManagerRunShell" 
        }
```

This condition element applies only to the default `SSM-SessionManagerRunShell` configuration document, and only when a user doesn’t specify a configuration document name in an CLI `[start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html)` command\. In other words, it ensures that the user has been granted access to `SSM-SessionManagerRunShell` when they run the following command:

```
aws ssm start-session --target i-02573cafcfEXAMPLE
```

For an example of specifying a Session Manager configuration document in a user’s IAM policy, see [Quickstart End User Policies for Session Manager](getting-started-restrict-access-quickstart.md#restrict-access-quickstart-end-user)\.

**Other Scenarios**  
Using the default `SSM-SessionManagerRunShell` configuration document is the only case when a document name can be omitted from the `[start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html)` CLI command\. In other cases, the document name must be specified, and the system checks whether the user has been granted explicit access to the configuration document they specify\. 

For example, if a user specifies the name of a custom configuration document you have created, the user’s IAM policy must grant them permission to access that document\. 

If a user runs a command to start a session using SSH, the user’s policy must grant them access to the `AWS-StartSSHSession` configuration document\. 

**Note**  
In order to start a session using SSH, configuration steps must be completed on both the target instance and the user's local machine\. For information, see [\(Optional\) Enable SSH Connections Through Session Manager](session-manager-getting-started-enable-ssh-connections.md)\.