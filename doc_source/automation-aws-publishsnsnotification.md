# AWS\-PublishSNSNotification<a name="automation-aws-publishsnsnotification"></a>

**Description**

Publish a notification to Amazon SNS\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-PublishSNSNotification)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this runbook\.
+ Message

  Type: String

  Description: \(Required\) The message to include in the SNS notification\.
+ TopicArn

  Type: String

  Description: \(Required\) The ARN of the SNS topic to publish the notification to\.