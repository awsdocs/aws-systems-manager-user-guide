# AWSConfigRemediation\-DeleteIAMRole<a name="automation-aws-delete-iam-role"></a>

**Description**

The AWSConfigRemediation\-DeleteIAMRole runbook deletes the AWS Identity and Access Management \(IAM\) role you specify\. This automation does not delete instance profiles associated with the IAM role, or service\-linked roles\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DeleteIAMRole)

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
+ IAMRoleID

  Type: String

  Description: \(Required\) The ID of the IAM role you want to delete\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `iam:DeleteRole`
+ `iam:DeleteRolePolicy`
+ `iam:GetRole`
+ `iam:ListAttachedRolePolicies`
+ `iam:ListInstanceProfilesForRole`
+ `iam:ListRolePolicies`
+ `iam:ListRoles`
+ `iam:RemoveRoleFromInstanceProfile`

**Document Steps**
+ aws:executeScript \- Gathers the name of the IAM role you specify in the `IAMRoleID` parameter\.
+ aws:executeScript \- Gathers policies and instance profiles associated with the IAM role\.
+ aws:executeScript \- Deletes attached policies\.
+ aws:executeScript \- Deletes the IAM role and verifies the role has been deleted\.