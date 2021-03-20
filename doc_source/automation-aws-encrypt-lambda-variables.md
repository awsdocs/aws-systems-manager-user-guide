# AWSConfigRemediation\-EncryptLambdaEnvironmentVariablesWithCMK<a name="automation-aws-encrypt-lambda-variables"></a>

**Description**

The AWSConfigRemediation\-EncryptLambdaEnvironmentVariablesWithCMK runbook encrypts, at rest, the environment variables for the AWS Lambda \(Lambda\) function you specify using an AWS Key Management Service \(AWS KMS\) customer master key \(CMK\)\. This runbook should only be used as a baseline to ensure that your Lambda function's environment variables are encrypted according to minimum recommended security best practices\. We recommend encrypting multiple functions with different CMKs\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EncryptLambdaEnvironmentVariablesWithCMK)

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

  Description: \(Required\) The name or ARN of the Lambda function whose environment variables you want to encrypt\.
+ KMSKeyArn

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS KMS CMK you want to use to encrypt your Lambda function's environment variables\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `lambda:GetFunctionConfiguration `
+ `lambda:UpdateFunctionConfiguration`

**Document Steps**
+ aws:waitForAwsResourceProperty \- Waits for the `LastUpdateStatus` property to be `Successful`\.
+ aws:executeAwsApi \- Encrypts the environment variables for the Lambda function you specify in the `FunctionName` parameter using the AWS KMS CMK you specify in the `KMSKeyArn` parameter\.
+ aws:assertAwsResourceProperty \- Confirms encryption is enabled on the environment variables for your Lambda function\.