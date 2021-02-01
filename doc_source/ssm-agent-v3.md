# SSM Agent version 3<a name="ssm-agent-v3"></a>

On September 21, 2020, AWS Systems Manager released SSM Agent version 3\.0\. Note the following important details about this release:
+ Version 3\.0 is backward compatible with version 2\.x\.
+ If you configured your managed instances to automatically update SSM Agent by using target version `$LATEST` \(the default configuration for auto\-update\), then Systems Manager automatically updates the agent on your instances to version 3\.0 and removes version 2\.x\.

  If you manually download SSM Agent, the system installs version 2\.x\.
+ When you update to version 3\.0, the system renames `amazon-ssm-agent` on your managed instance to `ssm-agent-worker`\. The update then installs a new binary named `amazon-ssm-agent`\. The new binary functions as a process manager for `ssm-agent-worker`\. The `ssm-agent-worker` binary communicates directly with Systems Manager to process requests\. 
**Important**  
Customers running SSM Agent in secure environments must add `ssm-agent-worker` to the allow list in your computing environment before upgrading\.
+ Version 2\.x is still supported, but it will be deprecated\.
+ Amazon Machine Images \(AMIs\) install version 2\.x until 2\.x is deprecated\. 
+ With version 3\.0, SSM Agent start and update events are logged on the instance\. For information about viewing SSM Agent log files, see [Viewing SSM Agent logs](sysman-agent-logs.md)\. 
+ You can use Amazon CloudWatch to perform actions on SSM Agent when the system logs a start or update event\. For more information, see [Create a CloudWatch Alarm Based on a Static Threshold](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ConsoleAlarms.html) in the *Amazon CloudWatch User Guide*\.
+ With version 3\.0, there is no change to the following:
  + Minimum AWS Identity and Access Management \(IAM\) permissions, the credential chain, or the `ssm-user` creation process
  + Supported platforms, log location, or debug logging
  + Command processing or SSM plugin support
  + The proxy configuration process
  + Windows Registry keys