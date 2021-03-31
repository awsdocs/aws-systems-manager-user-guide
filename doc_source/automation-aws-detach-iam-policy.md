# AWSConfigRemediation\-DetachIAMPolicy<a name="automation-aws-detach-iam-policy"></a>

**Description**

The AWSConfigRemediation\-DetachIAMPolicy runbook detaches the AWS Identity and Access Management \(IAM\) policy you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DetachIAMPolicy)

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

  Description: \(Required\) The ID of the IAM policy you want to detach\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `config:GetResourceConfigHistory`
+ `config:ListDiscoveredResources`
+ `iam:DetachGroupPolicy`
+ `iam:DetachRolePolicy`
+ `iam:DetachUserPolicy`
+ `iam:GetPolicy`
+ `iam:ListEntitiesForPolicy`

**Document Steps**
+ aws:executeScript \- Detaches the IAM policy from all resources\.