# AWS\-PublishSNSNotification<a name="automation-aws-publishsnsnotification"></a>

**Description**

Publish a notification to Amazon SNS\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-PublishSNSNotification)

**Document Type**

Automation

**Owner**

Amazon

**Platform\(s\)**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ Message

  Type: String

  Description: \(Required\) The message to include in the SNS notification\.
+ TopicArn

  Type: String

  Description: \(Required\) The ARN of the SNS topic to publish the notification to\.