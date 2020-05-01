# AWS\-RebootRDSInstance<a name="automation-aws-rebootrdsinstance"></a>

**Description**

Reboot an Amazon Relational Database Service \(Amazon RDS\) instance\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-RebootRDSInstance)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ InstanceId

  Type: String

  Description: \(Required\) The ID of the Amazon RDS instance that you want to reboot\.