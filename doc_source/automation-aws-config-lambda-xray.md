# AWSConfigRemediation\-ConfigureLambdaFunctionXRayTracing<a name="automation-aws-config-lambda-xray"></a>

**Description**

The AWSConfigRemediation\-ConfigureLambdaFunctionXRayTracing runbook enables AWS X\-Ray live tracing on the AWS Lambda function you specify in the `FunctionName` parameter\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-ConfigureLambdaFunctionXRayTracing)

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
+ FunctionName

  Type: String

  Description: \(Required\) The name or ARN of the Lambda function to enable tracing on\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `lambda:UpdateFunctionConfiguration`
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`

**Document Steps**
+ aws:executeAwsApi \- Enables X\-Ray tracing on the Lambda function you specify in the `FunctionName` parameter\.
+ aws:assertAwsResourceProperty \- Verifies that X\-Ray tracing has been enabled on the Lambda function\.

**Outputs**

UpdateLambdaConfig\.UpdateFunctionConfigurationResponse \- Response from the `UpdateFunctionConfiguration` API call\.