# AWSConfigRemediation\-RemoveUserPolicies<a name="automation-aws-remove-user-policies"></a>

**Description**

The AWSConfigRemediation\-RemoveUserPolicies runbook deletes the AWS Identity and Access Management \(IAM\) inline policies and detaches any managed policies attached to the IAM user you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-RemoveUserPolicies)

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
+ IAMUserID

  Type: String

  Description: \(Required\) The ID of the IAM user you want to remove policies from\.
+ PolicyType

  Type: String

  Valid values: All \| Inline \| Managed

  Default: All

  Description: \(Required\) The type of IAM policies you want to remove from the IAM user\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `iam:DeleteUserPolicy`
+ `iam:DetachUserPolicy`
+ `iam:ListAttachedUserPolicies`
+ `iam:ListUserPolicies`
+ `iam:ListUsers`

**Document Steps**
+ aws:executeScript \- Deletes and detaches IAM policies from the IAM user you specify in the `IAMUserID` parameter\.