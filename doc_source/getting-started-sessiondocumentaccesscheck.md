# Enforce a session document permission check for the AWS CLI<a name="getting-started-sessiondocumentaccesscheck"></a>

When you configure Session Manager for your account, the system creates a Session type document `SSM-SessionManagerRunShell`\. This document stores your session preferences, such as whether session data is saved in an Amazon Simple Storage Service \(Amazon S3\) bucket or Amazon CloudWatch Logs log group, whether session data is encrypted using AWS Key Management Service \(AWS KMS\), and whether Run As support is allowed for your sessions\. The following is an example\.

```
{
  "schemaVersion": "1.0",
  "description": "Document to hold regional settings for Session Manager",
  "sessionType": "Standard_Stream",
  "inputs": {
    "s3BucketName": "doc-example-bucket",
    "s3KeyPrefix": "BucketPrefix",
    "s3EncryptionEnabled": true,
    "cloudWatchLogGroupName": "LogGroupName",
    "cloudWatchEncryptionEnabled": true,
    "kmsKeyId": "kms-key",
    "runAsEnabled": true,
    "runAsDefaultUser": "RunAsUser"
  }
}
```

By default, when a user in your account has been granted permission to start sessions by their AWS Identity and Access Management \(IAM\) policy, they are also granted access to the `SSM-SessionManagerRunShell` SSM document\. This means that when they use the AWS CLI to run the `[start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html)` command, and they don't specify a document in the `--document-name` option, the system uses `SSM-SessionManagerRunShell` and launches the session\. The session starts even if the user’s IAM policy doesn’t grant explicit permission to access the `SSM-SessionManagerRunShell` document\.

For example, the following command doesn’t specify a Session document\.

```
aws ssm start-session \
    --target i-02573cafcfEXAMPLE
```

The following example specifies the default Session Manager Session document\.

```
aws ssm start-session \
    --document-name SSM-SessionManagerRunShell \
    --target i-02573cafcfEXAMPLE
```

To restrict access to the default or any Session document, you can add a condition element to the user's IAM policy that validates whether the user has explicit access to a Session document\. When this condition is applied, the user must specify a value for the `--document-name` option of the `[start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html)` AWS CLI command\. This value is either the default Session Manager Session document or a custom Session document you created\. The following condition element, when added to the `ssm:StartSession` action in the IAM policy, performs a session document access check\.

```
"Condition": {
    "BoolIfExists": {
        "ssm:SessionDocumentAccessCheck": "true"
    }
}
```

With this condition element set to `true`, explicit access to a Session document must be granted in the IAM policy for the user to start a session\. To ensure the condition element is enforced, it must be included in all policy statements which allow the `ssm:StartSession` action\. The following is an example\.

```
{
    "Effect": "Allow",
    "Action": [
        "ssm:StartSession"
    ],
    "Resource": [
        "arn:aws:ec2:us-west-2:123456789012:instance/i-02573cafcfEXAMPLE",
        "arn:aws:ssm:us-west-2:123456789012:document/SSM-SessionManagerRunShell"
    ],
    "Condition": {
        "BoolIfExists": {
            "ssm:SessionDocumentAccessCheck": "true"
        }
    }
}
```

If the `SessionDocumentAccessCheck` condition element is set to `false`, it will not affect the evaluation of the IAM policy\. That means if the `SessionDocumentAccessCheck` condition element is set to `true`, and you specify a document name in the `Resource`, you must provide the specified document name when starting a session\. If you provide a different document name when starting a session, the request fails\.

If the `SessionDocumentAccessCheck` condition element is set to `false`, and a document name is not specified in the `Resource`, you do not need to provide a document name when you start a session\. By default, the `SSM-SessionManagerRunShell` document is used in the request\.

For an example of specifying a Session Manager Session document in an IAM policy, see [Quickstart end user policies for Session Manager](getting-started-restrict-access-quickstart.md#restrict-access-quickstart-end-user)\.

**Other scenarios**  
Using the default `SSM-SessionManagerRunShell` session document is the only case when a document name can be omitted from the `[start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html)` CLI command\. In other cases, the user must specify a value for the `--document-name` option of the `[start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html)` AWS CLI command\. The system checks whether the user has explicit access to the Session document they specify\.

For example, if a user specifies the name of a custom Session document you created, the user’s IAM policy must grant them permission to access that document\. 

If a user runs a command to start a session using SSH, the user’s policy must grant them access to the `AWS-StartSSHSession` session document\. 

**Note**  
To start a session using SSH, configuration steps must be completed on both the target managed node and the user's local machine\. For information, see [\(Optional\) Enable and control permissions for SSH connections through Session Manager](session-manager-getting-started-enable-ssh-connections.md)\.