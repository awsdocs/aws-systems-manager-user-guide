# AWS\-RestartEC2Instance<a name="automation-aws-restartec2instance"></a>

**Description**

Restart one or more EC2 instances\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-RestartEC2Instance)

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

  Type: StringList

  Description: \(Required\) EC2 instance\(s\) to restart