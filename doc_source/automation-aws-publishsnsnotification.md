# AWS\-PublishSNSNotification<a name="automation-aws-publishsnsnotification"></a>

**Description**

Publish a notification to Amazon SNS\.

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
+ TopicARN

  Type: String

  Description: \(Required\) The ARN of the SNS topic to publish the notification to\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-PublishSNSNotification --parameters Message=message,TopicARN=arn:aws:sns:us-east-1:123456789012:SNSTopicARN
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

aws:executeAwsApi \- sns:Publish

**Outputs**

None