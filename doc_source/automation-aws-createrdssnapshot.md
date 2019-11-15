# AWS\-CreateRdsSnapshot<a name="automation-aws-createrdssnapshot"></a>

**Description**

Create an Amazon Relational Database Service \(Amazon RDS\) snapshot for an Amazon RDS instance\.

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

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-CreateRdsSnapshot --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

createRDSSnapshot – Creates the RDS snapshot and returns the snapshot ID\.

verifyRDSSnapshot – Checks that the snapshot created in the previous step exists\.

**Outputs**

createRDSSnapshot\.SnapshotId – The ID of the created snapshot\.