# AWSConfigRemediation\-ReplaceIAMInlinePolicy<a name="automation-aws-replace-iam-policy"></a>

**Description**

The AWSConfigRemediation\-ReplaceIAMInlinePolicy runbook replaces an inline AWS Identity and Access Management \(IAM\) policy with a replicated managed IAM policy\. For an inline policy attached to an IAM user, group, or role, the inline policy permissions are cloned into a managed IAM policy\. The managed IAM policy is added to the resource, and the inline policy is removed\. AWS Config must be enabled in the AWS Region where you run this automation\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-ReplaceIAMInlinePolicy)

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
+ InlinePolicyName

  Type: StringList

  Description: \(Required\) The inline IAM policy you want to replace\.
+ ResourceId

  Type: String

  Description: \(Required\) The ID of the IAM user, group, or role whose inline policy you want to replace\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `iam:AttachGroupPolicy`
+ `iam:AttachRolePolicy`
+ `iam:AttachUserPolicy`
+ `iam:CreatePolicy`
+ `iam:CreatePolicyVersion`
+ `iam:DeleteGroupPolicy`
+ `iam:DeleteRolePolicy`
+ `iam:DeleteUserPolicy`
+ `iam:GetGroupPolicy`
+ `iam:GetRolePolicy`
+ `iam:GetUserPolicy`
+ `iam:ListGroupPolicies`
+ `iam:ListRolePolicies`
+ `iam:ListUserPolicies`

**Document Steps**
+ aws:executeScript \- Replace the inline IAM policy with an AWS replicated policy on the resource that you specify\.