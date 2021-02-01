# AWSConfigRemediation\-EnableRedshiftClusterEnhancedVPCRouting<a name="automation-aws-enable-redshift-enhanced-routing"></a>

**Description**

The AWSConfigRemediation\-EnableRedshiftClusterEnhancedVPCRouting runbook enables enhanced virtual private cloud \(VPC\) routing for the Amazon Redshift cluster you specify\. For information about enhanced VPC routing, see [Amazon Redshift enhanced VPC routing](https://docs.aws.amazon.com/redshift/latest/gsg/enhanced-vpc-routing.html) in the *Amazon Redshift Cluster Management Guide*\.

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
+ ClusterIdentifer

  Type: String

  Description: \(Required\) The unique identifier of the cluster you want to enable enhanced VPC routing on\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `redshift:DescribeClusters`
+ `redshift:ModifyCluster`

**Document Steps**
+ aws:executeAwsApi \- Enables enhanced VPC routing on the cluster specified in the `ClusterIdentifer` parameter\.
+ assertAwsResourceProperty \- Confirms enhanced VPC routing was enabled on the cluster\.