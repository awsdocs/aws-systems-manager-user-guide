# AWS\-DeleteSnapshot<a name="automation-aws-deletesnapshot"></a>

**Description**

Delete a snapshot of an Amazon EBS volume\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-DeleteSnapshot)

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
+ SnapshotId

  Type: String

  Description: \(Required\) The ID of the EBS snapshot\.