# AWS\-StartEC2Instance<a name="automation-aws-startec2instance"></a>

**Description**

Start one or more EC2 instances\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-StartEC2Instance)

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

  Description: \(Required\) EC2 instances to start\.

**Outputs**

The automation execution has no output\.