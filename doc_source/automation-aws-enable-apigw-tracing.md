# AWSConfigRemediation\-EnableAPIGatewayTracing<a name="automation-aws-enable-apigw-tracing"></a>

**Description**

The AWSConfigRemediation\-EnableAPIGatewayTracing runbook enables tracing on an Amazon API Gateway \(API Gateway\) stage\. AWS Config must be enabled in the AWS Region where you run this automation\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableAPIGatewayTracing)

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

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the API Gateway stage you want to enable tracing on\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `apigateway:GetStage`
+ `apigateway:UpdateStage`

**Document Steps**
+ aws:executeScript \- Enables tracing on the API Gateway stage specified in the `StageArn` parameter\.