# AWSConfigRemediation\-DeleteRDSClusterSnapshot<a name="automation-aws-delete-rds-cluster-snap"></a>

**Description**

The AWSConfigRemediation\-DeleteRDSClusterSnapshot runbook deletes the given Amazon Relational Database Service \(Amazon RDS\) cluster snapshot\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableRDSInstanceBackup)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ DBClusterSnapshotId

  Type: String

  Description: \(Required\) The Amazon RDS cluster snapshot identifier to be deleted\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `rds:DeleteDBClusterSnapshot`
+ `rds:DescribeDBClusterSnapshots`

**Document Steps**
+ aws:branch \- Checks if the cluster snapshot is in the `available` state\. If it is not available, the flow ends\.
+ aws:executeAwsApi \- Deletes the given Amazon RDS cluster snapshot using the database \(DB\) cluster snapshot identifier\.
+ aws:executeScript \- Verifies that the given Amazon RDS cluster snapshot was deleted\.