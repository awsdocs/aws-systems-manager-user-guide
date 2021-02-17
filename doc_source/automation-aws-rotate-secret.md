# AWSConfigRemediation\-RotateSecret<a name="automation-aws-rotate-secret"></a>

**Description**

The AWSConfigRemediation\-RotateSecret runbook rotates a secret stored in AWS Secrets Manager\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-RotateSecret)

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
+ RotationInterval

  Type: Interval

  Valid values: 1\-365

  Description: \(Required\) The number of days between rotations of the secret\.
+ RotationLambdaArn

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Lambda funtion that can rotate the secret\.
+ SecretId

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the secret you want to rotate\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `lambda:InvokeFunction`
+ `secretsmanager:DescribeSecret`
+ `secretsmanager:RotateSecret`

**Document Steps**
+ aws:executeAwsApi \- Rotates the secret you specify in the `SecretId` parameter\.
+ aws:executeScript \- Verifies rotation has been enabled on the secret\.