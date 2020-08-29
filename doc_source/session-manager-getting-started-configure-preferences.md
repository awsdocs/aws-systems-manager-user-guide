# Step 4: Configure session preferences<a name="session-manager-getting-started-configure-preferences"></a>

An IAM user with administrator permissions can do the following:
+ Enable Run As support for Linux instances\. This makes it possible to start sessions using the credentials of a specified operating system user instead of the credentials of a system\-generated `ssm-user` account that Session Manager can create on a managed instance\.
+ Configure Session Manager to use AWS KMS key encryption to provide additional protection to the data transmitted between client machines and managed instances\.
+ Configure Session Manager to create and send session history logs to an Amazon Simple Storage Service \(Amazon S3\) bucket or an Amazon CloudWatch Logs log group\. The stored log data can then be used to audit or report on the session connections made to your instances and the commands run on them during the sessions\.

**Note**  
Before a user can update Session Manager preferences, they must have been granted the specific permissions that will let them make these updates, if they do not possess them already\. Without these permissions, the user can't configure logging options or set other session preferences for your account\.

**Topics**
+ [Grant or deny a user permissions to update Session Manager preferences](preference-setting-permissions.md)
+ [Enable run as support for Linux instances](session-preferences-run-as.md)
+ [Enable AWS KMS key encryption of session data \(console\)](session-preferences-enable-encryption.md)
+ [Create Session Manager preferences \(command line\)](getting-started-create-preferences-cli.md)
+ [Update Session Manager preferences \(command line\)](getting-started-configure-preferences-cli.md)

For information about using the Systems Manager console to configure options for logging session data, see the following topics\.
+  [Logging session data using Amazon S3 \(console\)](session-manager-logging-auditing.md#session-manager-logging-auditing-s3) 
+  [Logging session data using Amazon CloudWatch Logs \(console\)](session-manager-logging-auditing.md#session-manager-logging-auditing-cloudwatch-logs) 