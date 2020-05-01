# AWS\-CopySnapshot<a name="automation-aws-copysnapshot"></a>

**Description**

Copy a snapshot of an Amazon Elastic Block Store \(Amazon EBS\) volume\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-CopySnapshot)

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
+ Description

  Type: String

  Description: \(Optional\) A description for the Amazon EBS snapshot\.
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role assumed by Lambda\.
+ SnapshotId

  Type: String

  Description: \(Required\) The ID of the Amazon EBS snapshot to copy\.
+ SourceRegion

  Type: String

  Description: \(Required\) The region where the source snapshot currently exists\.