# AWSConfigRemediation\-RevokeUnusedIAMUserCredentials<a name="automation-aws-revoke-iam-user"></a>

**Description**

The AWSConfigRemediation\-RevokeUnusedIAMUserCredentials runbook revokes unused AWS Identity and Access Management \(IAM\) passwords and active access keys\. This runbook also deactivates expired access keys, and deletes expired login profiles\. AWS Config must be enabled in the AWS Region where you run this automation\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-RevokeUnusedIAMUserCredentials)

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
+ IAMResourceId

  Type: String

  Description: \(Required\) The ID of the IAM resource you want to revoke unused credentials from\.
+ MaxCredentialUsageAge

  Type: String

  Default: 90

  Description: \(Required\) The number of days within which the credential must have been used\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `config:ListDiscoveredResources`
+ `iam:DeleteAccessKey`
+ `iam:DeleteLoginProfile`
+ `iam:GetAccessKeyLastUsed`
+ `iam:GetLoginProfile`
+ `iam:GetUser`
+ `iam:ListAccessKeys`
+ `iam:UpdateAccessKey`

**Document Steps**
+ aws:executeScript \- Revokes IAM credentials for the user specified in the `IAMResourceId` parameter\. Expired access keys are deactivated, and expired login profiles are deleted\.