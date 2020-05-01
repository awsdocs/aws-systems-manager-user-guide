# AWS\-StartRDSInstance<a name="automation-aws-startrdsinstance"></a>

**Description**

Start an Amazon Relational Database Service \(Amazon RDS\) instance\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-StartRDSInstance)

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

  Description: \(Required\) ID of the Amazon RDS instance to start\.