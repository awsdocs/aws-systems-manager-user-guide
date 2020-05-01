# AWS\-CreateDynamoDBBackup<a name="automation-aws-createdynamodbbackup"></a>

**Description**

Create a backup of an Amazon DynamoDB table\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-CreateDynamoDBBackup)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

This document is *not* restricted to specific operating system\.

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ BackupName

  Type: String

  Description: \(Required\) Name of the backup to create\.
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf\. If not specified a transient role will be created to run the Lambda function\.
+ TableName

  Type: String

  Description: \(Required\) Name of the DynamoDB table\.