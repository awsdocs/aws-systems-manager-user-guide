# AWSConfigRemediation\-DeleteUnusedIAMGroup<a name="automation-aws-delete-iam-group"></a>

**Description**

The AWSConfigRemediation\-DeleteUnusedIAMGroup automation document deletes an IAM group that does not contain any IAM users\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DeleteUnusedIAMGroup)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ GroupName

  Type: String

  Description: \(Required\) The name of the IAM group that you want to delete\.

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:ExecuteAutomation`
+ `ssm:GetAutomationExecution`
+ `iam:DeleteGroup`
+ `iam:DeleteGroupPolicy`
+ `iam:DetachGroupPolicy`

**Document Steps**
+ aws:executeScript \- Removes managed and inline IAM policies attached to the target IAM group, and then deletes the IAM group\.