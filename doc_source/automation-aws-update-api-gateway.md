# AWSConfigRemediation\-UpdateAPIGatewayMethodCaching<a name="automation-aws-update-api-gateway"></a>

**Description**

The AWSConfigRemediation\-UpdateAPIGatewayMethodCaching runbook updates the cache method setting for an Amazon API Gateway stage resource\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableRDSInstanceBackup)

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
+ CachingAuthorizedMethods

  Type: StringList

  Description: \(Required\) The methods authorized to have caching enabled\. The list must be some combination of `DELETE`, `GET`, `HEAD`, `OPTIONS`, `PATCH`, `POST`, and `PUT`\. Caching is enabled for selected methods and disabled for non\-selected methods\. Caching is enabled for all methods if `ANY` is selected and is disabled for all methods if `NONE` is selected\.
+ StageArn

  Type: String

  Description: \(Required\) The API Gateway stage ARN for the `REST` API\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `apigateway:UpdateStage`
+ `apigateway:GetStage`

**Document Steps**
+ aws:executeScript \- Accepts the stage resource ID as input, updates the cache method setting for anAPI Gateway stage using the `UpdateStage` API action, and verifies the update\.