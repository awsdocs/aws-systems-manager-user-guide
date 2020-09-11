# AWS\-DeleteDynamoDBTableBackups<a name="automation-aws-deletedynamodbtablebackups"></a>

**Description**

Delete DynamoDB table backups based on retention days or count\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-DeleteDynamoDBTableBackups)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf\. If not specified a transient role will be created to run the Lambda function\.
+ RetentionCount

  Type: String

  Default: 10

  Description: \(Optional\) The number of backups to retain for the table\. If more than the specified number of backup exist, the oldest backups beyond that number are deleted\. Either RetentionCount or RetentionDays can be used, not both\.
+ RetentionDays

  Type: String

  Description: \(Optional\) The number of days to retain backups for the table\. Backups older than the specified number of days are deleted\. Either RetentionCount or RetentionDays can be used, not both\.
+ TableName

  Type: String

  Description: \(Required\) Name of the DynamoDB table\.