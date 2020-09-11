# AWS\-CopySnapshot<a name="automation-aws-copysnapshot"></a>

**Description**

Copies a point\-in\-time snapshot of an Amazon Elastic Block Store \(Amazon EBS\) volume\. You can copy the snapshot within the same AWS Region or from one Region to another\. Copies of encrypted Amazon EBS snapshots remain encrypted\. Copies of unencrypted snapshots remain unencrypted\. To copy an encrypted snapshot that was shared from another account, you must have permissions for the AWS KMS customer master key \(CMK\) used to encrypt the snapshot\. Snapshots created by copying another snapshot have an arbitrary volume ID that should not be used for any purpose\.

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

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ Description

  Type: String

  Description: \(Optional\) A description for the Amazon EBS snapshot\.
+ SnapshotId

  Type: String

  Description: \(Required\) The ID of the Amazon EBS snapshot to copy\.
+ SourceRegion

  Type: String

  Description: \(Required\) The Region where the source snapshot currently exists\.

**Document Steps**

copySnapshot \- Copies a snapshot of an Amazon EBS volume\.

**Outputs**

copySnapshot\.SnapshotId \- The ID of the new snapshot\.