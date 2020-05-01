# AWS\-DeleteDynamoDBBackup<a name="automation-aws-deletedynamodbbackup"></a>

**Description**

Delete the backup of an Amazon DynamoDB table\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-DeleteDynamoDBBackup)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ BackupArn

  Type: String

  Description: \(Required\) ARN of the DynamoDB table backup to delete\.