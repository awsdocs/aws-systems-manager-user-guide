# AWSConfigRemediation\-DeleteAPIGatewayStage<a name="automation-aws-delete-apigw-stage"></a>

**Description**

The AWSConfigRemediation\-DeleteAPIGatewayStage runbook deletes an Amazon API Gateway \(API Gateway\) stage\. AWS Config must be enabled in the AWS Region where you run this automation\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DeleteAPIGatewayStage)

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
+ StageArn

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the API Gateway stage you want to delete\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `config:GetResourceConfigHistory`
+ `apigateway:GetStage`
+ `apigateway:DeleteStage`

**Document Steps**
+ aws:executeScript \- Deletes the API Gateway stage specified in the `StageArn` parameter\.