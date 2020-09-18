# Step 8: \(Optional\) Set up integrations with other AWS services<a name="setup-integrations"></a>

AWS Systems Manager integrates with a number of other AWS services\. In most cases, you set up an integration after you decide to incorporate the service into your Systems Manager operations\. For example:
+ [Referencing AWS Secrets Manager secrets from Parameter Store parameters](integration-ps-secretsmanager.md)
+ [Using Chef InSpec profiles with Systems Manager Compliance](integration-chef-inspec.md)

You can use some AWS services immediately to compile log data for later troubleshooting and analysis\. You can also use AWS services to monitor and quickly respond to changes in your Systems Manager environment\. Therefore, we recommend that you set up the following resources as part of your initial Systems Manager setup process\. 

**Amazon EventBridge** and **Amazon Simple Notification Service** – EventBridge lets you set up rules to detect when changes happen to AWS resources that you specify\. You can configure EventBridge to log status execution changes of the commands users in your account send using Systems Manager\. You can create a rule to detect when a user in your organization starts or stops a session in Session Manager\. You can also configure an EventBridge event to trigger other actions in your AWS environment\. For more information, see the following topics:
+ [Understanding command statuses](monitor-commands.md)
+ [Configuring EventBridge for Systems Manager events](monitoring-systems-manager-events.md)
+ [ Monitoring session activity using Amazon EventBridge \(console\) ](session-manager-logging-auditing.md#session-manager-logging-auditing-eventbridge-events)

**Amazon Simple Storage Service \(Amazon S3\)**  
Run Command command output in the Systems Manager console is truncated after 2,500 characters\. In order to access complete command output logs, you can store Systems Manager output in an Amazon Simple Storage Service \(Amazon S3\) bucket, and then use this output later for auditing or troubleshooting\. You specify whether to save command output to an S3 bucket each time you run a command\. You can also create an Amazon S3 key prefix \(a subfolder\) to help you organize the log output\. For more information, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\. 

**Amazon CloudWatch Logs \(CloudWatch Logs\)**  
As an alternative to storing command output in an S3 bucket, you can send output to an Amazon CloudWatch Logs log group\. If you specify CloudWatch Logs as the output target, Run Command periodically sends all command output and error logs to CloudWatch Logs\. You can monitor output logs in near real\-time, search for specific phrases, values, or patterns, and create alarms based on the search\. For more information, see [Configuring Amazon CloudWatch Logs for Run Command](sysman-rc-setting-up-cwlogs.md)\.