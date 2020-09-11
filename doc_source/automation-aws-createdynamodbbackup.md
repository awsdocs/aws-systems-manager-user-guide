# AWS\-CreateDynamoDBBackup<a name="automation-aws-createdynamodbbackup"></a>

**Description**

Create a backup of an Amazon DynamoDB table\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-CreateDynamoDBBackup)

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
+ BackupName

  Type: String

  Description: \(Required\) Name of the backup to create\.
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf\. If not specified a transient role will be created to run the Lambda function\.
+ TableName

  Type: String

  Description: \(Required\) Name of the DynamoDB table\.