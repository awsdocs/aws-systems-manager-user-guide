# AWSConfigRemediation\-EnablePITRForDynamoDbTable<a name="automation-aws-enable-pitr-ddb"></a>

**Description**

The AWSConfigRemediation\-EnablePITRForDynamoDbTable runbook enables point\-in\-time recovery \(PITR\) on the Amazon DynamoDB table you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnablePITRForDynamoDbTable)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Databases

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ TableName

  Type: String

  Description: \(Required\) The name of the DynamoDB table to enable point\-in\-time recovery on\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `dynamodb:DescribeContinuousBackups `
+ `dynamodb:UpdateContinuousBackups`

**Document Steps**
+ aws:executeAwsApi \- Enables point\-in\-time recovery on the DynamoDB table you specify in the `TableName` parameter\.
+ aws:assertAwsResourceProperty \- Confirms point\-in\-time recovery is enabled on the DynamoDB table\.