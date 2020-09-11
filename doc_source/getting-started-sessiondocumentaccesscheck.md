# Enforce a session document permission check for the AWS CLI<a name="getting-started-sessiondocumentaccesscheck"></a>

When you configure Session Manager for your account, the system creates a `Session`\-type SSM document named `SSM-SessionManagerRunShell`\. This SSM document stores your session preferences, such as whether session data is saved in an S3 bucket or Amazon CloudWatch Logs log group, whether session data is encrypted using AWS Key Management Service, and whether Run As support is enabled for your sessions\. The following is an example\.

```
{
  "schemaVersion": "1.0",
  "description": "Document to hold regional settings for Session Manager",
  "sessionType": "Standard_Stream",
  "inputs": {
    "s3BucketName": "DOC-EXAMPLE-BUCKET",
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

By default, if a user in your account was granted permission in their IAM user policy to start sessions, that user has access to the `SSM-SessionManagerRunShell` SSM document\. This means that when they use the AWS CLI to run the `[start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html)` command, and they do not specify a document in the `--document-name` option, the system uses `SSM-SessionManagerRunShell` and launches the session\. The session starts even if the user’s IAM policy doesn’t grant explicit permission to access the `SSM-SessionManagerRunShell` document\.

For example, the following command doesn’t specify a session document\.

```
aws ssm start-session \
    --target i-02573cafcfEXAMPLE
```

The following example specifies the default Session Manager session document\.

```
aws ssm start-session \
    --document-name SSM-SessionManagerRunShell \
    --target i-02573cafcfEXAMPLE
```

To restrict access to the default or any session document, you can add a condition element to the user's IAM policy that validates whether the user has explicit access to a session document\. When this condition is applied, the user must specify a value for the `--document-name` option of the `[start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html)` AWS CLI command\. This value is either the default Session Manager session document or a custom session document you created\. The following condition element, when added to the `ssm:StartSession` action in the IAM policy, performs a session document access check\.

```
"Condition": {
    "BoolIfExists": {
        "ssm:SessionDocumentAccessCheck": "true"
    }
}
```

With this condition element set to `true`, explicit access to a session document must be granted in the IAM policy for the user to start a session\. The following is an example\.

```
{
    "Effect": "Allow",
    "Action": [
        "ssm:StartSession"
    ],
    "Resource": [
        "arn:aws:ec2:*:*:instance/instance-id",
        "arn:aws:ssm:region:account-id:document/SSM-SessionManagerRunShell"
    ] 
}
```

For an example of specifying a Session Manager session document in an IAM policy, see [Quickstart end user policies for Session Manager](getting-started-restrict-access-quickstart.md#restrict-access-quickstart-end-user)\.

**Other Scenarios**  
Using the default `SSM-SessionManagerRunShell` session document is the only case when a document name can be omitted from the `[start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html)` CLI command\. In other cases, the user must specify a value for the `--document-name` option of the `[start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html)` AWS CLI command\. The system checks whether the user has explicit access to the session document they specify\.

For example, if a user specifies the name of a custom session document you created, the user’s IAM policy must grant them permission to access that document\. 

If a user runs a command to start a session using SSH, the user’s policy must grant them access to the `AWS-StartSSHSession` session document\. 

**Note**  
To start a session using SSH, configuration steps must be completed on both the target instance and the user's local machine\. For information, see [\(Optional\) Enable SSH connections through Session Manager](session-manager-getting-started-enable-ssh-connections.md)\.