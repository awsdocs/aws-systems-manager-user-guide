# AWS\-CreateRdsSnapshot<a name="automation-aws-createrdssnapshot"></a>

**Description**

Create an Amazon Relational Database Service \(Amazon RDS\) snapshot for an Amazon RDS instance\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-CreateRdsSnapshot)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ DBInstanceIdentifier

  Type: String

  Description: \(Required\) The DBInstanceId ID of the RDS Instance to create Snapshot from\.
+ DBSnapshotIdentifier

  Type: String

  Description: \(Optional\) The DBSnapshotIdentifier ID of the RDS snapshot to create\.
+ InstanceTags

  Type: String

  Description: \(Optional\) Tags to create for instance\.
+ SnapshotTags

  Type: String

  Description: \(Optional\) Tags to create for snapshot\.
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.

**Document Steps**

createRDSSnapshot – Creates the RDS snapshot and returns the snapshot ID\.

verifyRDSSnapshot – Checks that the snapshot created in the previous step exists\.

**Outputs**

createRDSSnapshot\.SnapshotId – The ID of the created snapshot\.