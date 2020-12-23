# AWSConfigRemediation\-DeleteLambdaFunction<a name="automation-aws-delete-lambda"></a>

**Description**

The AWSConfigRemediation\-DeleteLambdaFunction runbook deletes the AWS Lambda function you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DeleteLambdaFunction)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ LambdaFunctionName

  Type: String

  Description: \(Required\) The name of the Lambda function that you want to delete\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:ExecuteAutomation`
+ `ssm:GetAutomationExecution`
+ `lambda:DeleteFunction`
+ `lambda:GetFunction`

**Document Steps**
+ aws:executeAwsApi \- Deletes the Lambda function specified in the `LambdaFunctionName` parameter\.
+ aws:executeScript \- Verifies the Lambda function has been deleted\.