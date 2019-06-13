# AWS\-DeleteDynamoDBBackup<a name="automation-aws-deletedynamodbbackup"></a>

**Description**

Delete the backup of an Amazon DynamoDB table\.

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

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-DeleteDynamoDbBackup --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

aws:executeAwsApi

**Outputs**

None