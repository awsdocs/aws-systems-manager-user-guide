# AWSConfigRemediation\-EnableCopyTagsToSnapshotOnRDSCluster<a name="automation-aws-enable-tags-snapshot-rds-cluster"></a>

**Description**

The AWSConfigRemediation\-EnableCopyTagsToSnapshotOnRDSCluster runbook enables the `CopyTagsToSnapshot` setting on the Amazon Relational Database Service \(Amazon RDS\) cluster you specify\. Enabling this setting copies all tags from the DB cluster to snapshots of the DB cluster\. The default is not to copy them\. AWS Config must be enabled in the AWS Region where you run this automation\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableCopyTagsToSnapshotOnRDSCluster)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Databases

**Parameters**
+ ApplyImmediately

  Type: Boolean

  Default: false

  Description: \(Optional\) If you specify `true` for this parameter, the modifications in this request and any pending modifications are asynchronously applied as soon as possible, regardless of the `PreferredMaintenanceWindow` setting for the DB cluster\.
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ DbClusterResourceId

  Type: String

  Description: \(Required\) The resource identifier for the DB cluster you want to enable the `CopyTagsToSnapshot` setting on\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `config:GetResourceConfigHistory`
+ `rds:DescribeDBClusters`
+ `rds:ModifyDBCluster`

**Document Steps**
+ aws:executeAwsApi \- Gathers the DB cluster identifier from the DB cluster resource identifier\.
+ aws:assertAwsResourceProperty \- Confirms the DB cluster is in an `AVAILABLE` state\.
+ aws:executeAwsApi \- Enables the `CopyTagsToSnapshot` setting on your DB cluster\.
+ aws:assertAwsResourceProperty \- Confirms the `CopyTagsToSnapshot` setting is enabled on your DB cluster\.