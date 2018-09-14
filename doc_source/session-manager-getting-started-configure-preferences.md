# Step 4: Configure Session Preferences<a name="session-manager-getting-started-configure-preferences"></a>

A system administrator can configure Session Manager to create and send session history logs to an Amazon Simple Storage Service \(Amazon S3\) bucket or an Amazon CloudWatch Log log group\. The stored log data can then be used to audit or report on the session connections made to your instances and the commands run on them during the sessions\. 

A user who does not have administrative access already must be granted to necessary permissions to update these Session Manager preferences before they can configure logging options or set other session preferences for your account\. For information about granting these permissions, see Session Manager preferences in your account, see [Grant or Deny a User Permissions to Update Session Manager Preferences](preference-setting-permissions.md)\.

For information about configuring Session Manager to store session logs, see the following topics:
+ [Log Session Data Using Amazon S3](session-manager-logging-auditing.md#session-manager-logging-auditing-s3)
+ [Log Session Data Using Amazon CloudWatch Logs](session-manager-logging-auditing.md#session-manager-logging-auditing-cloudwatch-logs)