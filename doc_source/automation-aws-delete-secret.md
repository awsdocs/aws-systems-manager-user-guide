# AWSConfigRemediation\-DeleteSecret<a name="automation-aws-delete-secret"></a>

**Description**

The AWSConfigRemediation\-RotateSecret runbook deletes a secret and all of the versions stored in AWS Secrets Manager\. You can optionally specify the recovery window during which you can restore the secret\. If you don't specify a value for the `RecoveryWindowInDays` parameter, the operation defaults to 30 days\.

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
+ RecoveryWindowInDays

  Type: Integer

  Valid values: 7\-30

  Default: 30

  Description: \(Optional\) The number of days which you can restore the secret\.
+ SecretId

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the secret you want to delete\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `secretsmanager:DeleteSecret`
+ `secretsmanager:DescribeSecret`

**Document Steps**
+ aws:executeAwsApi \- Deletes the secret you specify in the `SecretId` parameter\.
+ aws:executeScript \- Verifies the secret has been scheduled for deletion\.