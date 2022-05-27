# Working with Session Manager<a name="session-manager-working-with"></a>

You can use the AWS Systems Manager console, the Amazon Elastic Compute Cloud \(Amazon EC2\) console, or the AWS Command Line Interface \(AWS CLI\) to start sessions that connect you to the managed nodes your system administrator has granted you access to using AWS Identity and Access Management \(IAM\) policies\. Depending on your permissions, you can also view information about sessions, resume inactive sessions that haven't timed out, and end sessions\. After a session is established, it is not affected by IAM role session duration\. For information about limiting session duration with Session Manager, see [Specify an idle session timeout value](session-preferences-timeout.md) and [Specify maximum session duration ](session-preferences-max-timeout.md)\.

For more information about sessions, see [What is a session?](session-manager.md#what-is-a-session)

**Topics**
+ [\(Optional\) Install the Session Manager plugin for the AWS CLI](session-manager-working-with-install-plugin.md)
+ [Start a session](session-manager-working-with-sessions-start.md)
+ [End a session](session-manager-working-with-sessions-end.md)
+ [View session history](session-manager-working-with-view-history.md)