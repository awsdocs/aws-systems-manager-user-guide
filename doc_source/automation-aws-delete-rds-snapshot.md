# AWSConfigRemediation\-DeleteRDSInstanceSnapshot<a name="automation-aws-delete-rds-snapshot"></a>

**Description**

The AWSConfigRemediation\-DeleteRDSInstanceSnapshot runbook deletes the Amazon Relational Database Service \(Amazon RDS\) instance snapshot you specify\. Only snapshots in the `available` state are deleted\. This runbook does not support deleting snapshots from Amazon Aurora database instances\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DeleteRDSInstanceSnapshot)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Databases

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ DbSnapshotId

  Type: String

  Description: \(Required\) The ID of the snapshot you want to delete\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `rds:DeleteDBSnapshot`
+ `rds:DescribeDBSnapshots`

**Document Steps**
+ aws:executeAwsApi \- Gathers the state of the snapshot specified in the `DbSnapshotId` parameter\.
+ aws:assertAwsResourceProperty \- Confirms the state of the snapshot is `available`\.
+ aws:executeAwsApi \- Deletes the snapshot specified in the `DbSnapshotId` parameter\.
+ aws:executeScript \- Verifies the snapshot has been deleted\.