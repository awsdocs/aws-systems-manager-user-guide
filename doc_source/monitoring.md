# Monitoring Instances with AWS Systems Manager<a name="monitoring"></a>

SSM Agent writes information about executions, scheduled actions, errors, and health statuses to log files on each instance\. Manually connecting to an instance to view log files and troubleshoot an issue with SSM Agent is time\-consuming\. For more efficient instance monitoring, you can configure either the SSM Agent itself or the CloudWatch Agent to send this log data to Amazon CloudWatch Logs\. Using CloudWatch Logs, you can monitor log data in real\-time, search and filter log data by creating one or more metric filters, and archive and retrieve historical data when you need it\. For more information about CloudWatch Logs, see the *[Amazon CloudWatch Logs User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)*\.

Configuring an agent to send log data to Amazon CloudWatch Logs provides the following benefits:

+ Centralized log file storage for all of your SSM Agent log files\.

+ Quicker access to files to investigate errors\.

+ Indefinite log file retention \(configurable\)\.

+ Logs can be maintained and accessed regardless of the status of the instance\.

+ Access to other CloudWatch features such as metrics and alarms\.


+ [Send Logs to CloudWatch Logs \(SSM Agent\)](monitoring-ssm-agent.md)
+ [Send Logs to CloudWatch Logs \(CloudWatch Agent\)](monitoring-cloudwatch-agent.md)
+ [Log Systems Manager API Calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)