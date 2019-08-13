# Step 4: Configure Session Preferences<a name="session-manager-getting-started-configure-preferences"></a>

An IAM user with administrator permissions can do the following:
+ Enable Run As support for Linux instances\. This makes it possible to start sessions using the credentials of a specified operating system user instead of the credentials of a system\-generated `ssm-user` account that Session Manager can create on a managed instance\.
+ Configure Session Manager to use AWS KMS key encryption to provide additional protection to the data transmitted between client machines and managed instances\.
+ Configure Session Manager to create and send session history logs to an Amazon Simple Storage Service \(Amazon S3\) bucket or an Amazon CloudWatch Logs log group\. The stored log data can then be used to audit or report on the session connections made to your instances and the commands run on them during the sessions\.

**Note**  
Before a user can update Session Manager preferences, they must have been granted the specific permissions that will let them make these updates, if they do not possess them already\. Without these permissions, the user can't configure logging options or set other session preferences for your account\.

**Topics**
+ [Grant or Deny a User Permissions to Update Session Manager Preferences](preference-setting-permissions.md)
+ [Enable Run As Support for Linux Instances](session-preferences-run-as.md)
+ [Enable AWS KMS Key Encryption of Session Data \(Console\)](session-preferences-enable-encryption.md)
+ [Create Session Manager Preferences \(AWS CLI\)](getting-started-create-preferences-cli.md)
+ [Update Session Manager Preferences \(AWS CLI\)](getting-started-configure-preferences-cli.md)

For information about using the Systems Manager console to configure options for logging session data, see the following topics\.
+ [Logging Session Data Using Amazon S3 \(Console\)](session-manager-logging-auditing.md#session-manager-logging-auditing-s3)
+ [Logging Session Data Using Amazon CloudWatch Logs \(Console\)](session-manager-logging-auditing.md#session-manager-logging-auditing-cloudwatch-logs)