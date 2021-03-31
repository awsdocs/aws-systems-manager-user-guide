# AWSConfigRemediation\-DeleteRedshiftCluster<a name="automation-aws-delete-redshift"></a>

**Description**

The AWSConfigRemediation\-DeleteRedshiftCluster runbook deletes the Amazon Redshift cluster you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DeleteRedshiftCluster)

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
+ ClusterIdentifier

  Type: String

  Description: \(Required\) The ID of the Amazon Redshift cluster that you want to delete\.
+ SkipFinalClusterSnapshot

  Type: Boolean

  Default: False

  Description: \(Optional\) If set to `False`, the automation creates a snapshot before deleting the Amazon Redshift cluster\. If set to `True`, a final cluster snapshot is not created\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `redshift:DeleteCluster`
+ `redshift:DescribeClusters`

**Document Steps**
+ aws:branch \- Branches based on the value you specify for the `SkipFinalClusterSnapshot` parameter\.
+ aws:executeAwsApi \- Deletes the Amazon Redshift cluster specified in the `ClusterIdentifier` parameter\.
+ aws:assertAwsResourceProperty \- Verifies the Amazon Redshift cluster has been deleted\.