# AWSConfigRemediation\-CreateGuardDutyDetector<a name="automation-aws-enable-guard-detect"></a>

**Description**

The AWSConfigRemediation\-CreateGuardDutyDetector runbook creates an Amazon GuardDuty \(GuardDuty\) detector in the AWS Region where you run the automation\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-CreateGuardDutyDetector)

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

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `guardduty:CreateDetector`
+ `guardduty:GetDetector`

**Document Steps**
+ aws:executeAwsApi \- Creates a GuardDuty detector\.
+ aws:assertAwsResourceProperty \- Verifies the `Status` of the detector is `ENABLED`\.