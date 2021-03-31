# AWSConfigRemediation\-ModifyRedshiftClusterMaintenanceSettings<a name="automation-aws-modify-redshift-maintenance"></a>

**Description**

The AWSConfigRemediation\-ModifyRedshiftClusterMaintenanceSettings runbook modifies the maintenance settings for the Amazon Redshift cluster you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-ModifyRedshiftClusterMaintenanceSettings)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Databases

**Parameters**
+ AllowVersionUpgrade

  Type: Boolean

  Description: \(Required\) If set to `True`, major version upgrades are applied automatically to the cluster during the maintenance window\.
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ AutomatedSnapshotRetentionPeriod

  Type: Integer

  Valid values: 1\-35

  Description: \(Required\) The number of days automated snapshots are retained\.
+ ClusterIdentifer

  Type: String

  Description: \(Required\) The unique identifier of the cluster you want to enable enhanced VPC routing on\.
+ PreferredMaintenanceWindow

  Type: String

  Description: \(Required\) The weekly time range \(in UTC\) during which system maintenance can occur\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `redshift:DescribeClusters`
+ `redshift:ModifyCluster`

**Document Steps**
+ aws:executeAwsApi \- Modifies the maintenance settings for the cluster specified in the `ClusterIdentifer` parameter\.
+ assertAwsResourceProperty \- Confirms the modified maintenance settings were configured for the cluster\.