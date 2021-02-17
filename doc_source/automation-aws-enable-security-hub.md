# AWSConfigRemediation\-EnableSecurityHub<a name="automation-aws-enable-security-hub"></a>

**Description**

The AWSConfigRemediation\-EnableSecurityHub runbook enables AWS Security Hub \(Security Hub\) for the AWS account and AWS Region where you run the automation\. For information about Security Hub, see [What is AWS Security Hub?](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html) in the *AWS Security Hub User Guide*\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableSecurityHub)

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
+ EnableDefaultStandards

  Type: Boolean

  Default: True

  Description: \(Required\) If set to `True`, the default security standards designated by Security Hub are enabled\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `securityhub:DescribeHub`
+ `securityhub:EnableSecurityHub`
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`

**Document Steps**
+ aws:executeAwsApi \- Enables Security Hub in the current account and Region\.
+ aws:executeAwsApi \- Verifies that Security Hub has been enabled\.