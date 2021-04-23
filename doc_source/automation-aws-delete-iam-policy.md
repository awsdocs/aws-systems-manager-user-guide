# AWSConfigRemediation\-DeleteUnusedIAMPolicy<a name="automation-aws-delete-iam-policy"></a>

**Description**

The AWSConfigRemediation\-DeleteUnusedIAMPolicy runbook deletes an AWS Identity and Access Management \(IAM\) policy that is not attached to any IAM users, groups, or roles\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DeleteUnusedIAMPolicy)

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

  Description: \(Required\) The resource identifier of the IAM policy that you want to delete\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `config:GetResourceConfigHistory`
+ `config:ListDiscoveredResources`
+ `iam:DeletePolicy`
+ `iam:DeletePolicyVersion`
+ `iam:GetPolicy`
+ `iam:ListEntitiesForPolicy`
+ `iam:ListPolicyVersions`

**Document Steps**
+ aws:executeScript \- Deletes the policy you specify in the `IAMResourceId` parameter, and verifies the policy was deleted\.