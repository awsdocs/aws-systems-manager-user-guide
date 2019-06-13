# AWS\-DeleteDynamoDBTableBackups<a name="automation-aws-deletedynamodbtablebackups"></a>

**Description**

Delete DynamoDB table backups based on retention days or count\.

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

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-DeleteDynamoDbTableBackups --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

aws:createStack

aws:invokeLambdaFunction

aws:deleteStack

**Outputs**

deleteDynamoDbTableBackups\.Payload