# Step 4: Configure session preferences<a name="session-manager-getting-started-configure-preferences"></a>

An AWS Identity and Access Management \(IAM\) user with administrator permissions can do the following:
+ Enable Run As support for Linux instances\. This makes it possible to start sessions using the credentials of a specified operating system user instead of the credentials of a system\-generated `ssm-user` account that Session Manager can create on a managed instance\.
+ Configure Session Manager to use AWS Key Management Service \(AWS KMS\) key encryption to provide additional protection to the data transmitted between client machines and managed instances\.
+ Configure Session Manager to create and send session history logs to an Amazon Simple Storage Service \(Amazon S3\) bucket or an Amazon CloudWatch Logs log group\. The stored log data can then be used to audit or report on the session connections made to your instances and the commands run on them during the sessions\.
+ Configure session timeouts\. You can use this setting to specify when to end a session after a period of inactivity\.
+ Configure Session Manager to use configurable shell profiles\. These customizable profiles enable you to define preferences within sessions such as shell preferences, environment variables, working directories, and running multiple commands when a session is started\.

**Note**  
Before a user can update Session Manager preferences, they must have been granted the specific permissions that will let them make these updates, if they do not possess them already\. Without these permissions, the user can't configure logging options or set other session preferences for your account\.

**Topics**
+ [Grant or deny a user permissions to update Session Manager preferences](preference-setting-permissions.md)
+ [Specify an idle session timeout value](session-preferences-timeout.md)
+ [Enable configurable shell profiles](session-preferences-shell-config.md)
+ [Enable run as support for Linux instances](session-preferences-run-as.md)
+ [Enable AWS KMS key encryption of session data \(console\)](session-preferences-enable-encryption.md)
+ [Create Session Manager preferences \(command line\)](getting-started-create-preferences-cli.md)
+ [Update Session Manager preferences \(command line\)](getting-started-configure-preferences-cli.md)

For information about using the Systems Manager console to configure options for logging session data, see the following topics:
+  [Logging session data using Amazon S3 \(console\)](session-manager-logging.md#session-manager-logging-s3) 
+ [Streaming session data using Amazon CloudWatch Logs \(console\)](session-manager-logging.md#session-manager-logging-cwl-streaming)
+  [Logging session data using Amazon CloudWatch Logs \(console\)](session-manager-logging.md#session-manager-logging-cloudwatch-logs) 