# AWS\-CreateDynamoDBBackup<a name="automation-aws-createdynamodbbackup"></a>

**Description**

Create a backup of an Amazon DynamoDB table\.

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
+ BackupName

  Type: String

  Description: \(Required\) Name of the backup to create\.
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf\. If not specified a transient role will be created to run the Lambda function\.
+ TableName

  Type: String

  Description: \(Required\) Name of the DynamoDB table\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-CreateDynamoDbBackup --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

aws:createStack

aws:invokeLambdaFunction

aws:invokeLambdaFunction

aws:deleteStack

**Outputs**

createDynamoDbBackup\.Payload