# AWSConfigRemediation\-EnforceSSLOnlyConnectionsToRedshiftCluster<a name="automation-aws-enforce-redshift-ssl-only"></a>

**Description**

The AWSConfigRemediation\-EnforceSSLOnlyConnectionsToRedshiftCluster runbook requires incoming connections to use SSL for the Amazon Redshift cluster you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnforceSSLOnlyConnectionsToRedshiftCluster)

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

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `redshift:DescribeClusters`
+ `redshift:DescribeClusterParameters`
+ `redshift:ModifyClusterParameterGroup`

**Document Steps**
+ aws:executeAwsApi \- Gathers parameter details from the cluster specified in the `ClusterIdentifer` parameter\.
+ aws:executeAwsApi \- Enables the `require_ssl` setting on the cluster specified in the `ClusterIdentifer` parameter\.
+ aws:assertAwsResourceProperty \- Confirms the `require_ssl` setting was enabled on the cluster\.
+ aws:executeScript \- Verifies the `require_ssl` setting for the cluster\.