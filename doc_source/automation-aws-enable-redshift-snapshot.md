# AWSConfigRemediation\-EnableRedshiftClusterAutomatedSnapshot<a name="automation-aws-enable-redshift-snapshot"></a>

**Description**

The AWSConfigRemediation\-EnableRedshiftClusterAutomatedSnapshot runbook enables automated snapshots for the Amazon Redshift cluster you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableRedshiftClusterAutomatedSnapshot)

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
+ AutomatedSnapshotRetentionPeriod

  Type: Integer

  Valid values: 1\-35

  Description: \(Required\) The number of days that automated snapshots are retained\.
+ ClusterIdentifer

  Type: String

  Description: \(Required\) The unique identifier of the cluster you want to enable automated snapshots on\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `redshift:DescribeClusters`
+ `redshift:ModifyCluster`

**Document Steps**
+ aws:executeAwsApi \- Enables automation snapshots on the cluster specified in the `ClusterIdentifer` parameter\.
+ aws:waitForAwsResourceProperty \- Waits for the state of the cluster to change to `available`\.
+ aws:executeScript \- Confirms automated snapshots were enabled on the cluster\.