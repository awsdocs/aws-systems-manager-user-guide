# AWSConfigRemediation\-ModifyRedshiftClusterNodeType<a name="automation-aws-modify-redshift-cluster-node"></a>

**Description**

The AWSConfigRemediation\-ModifyRedshiftClusterNodeType runbook modifies the node type and number of nodes for the Amazon Redshift cluster you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-ModifyRedshiftClusterNodeType)

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
+ Classic

  Type: Boolean

  Description: \(Optional\) If set to `True`, the resize operation uses the classic resize process\.
+ ClusterIdentifier

  Type: String

  Description: \(Required\) The unique identifier of the cluster whose node type you want to modify\.
+ ClusterType

  Type: String

  Valid values: single\-node \| multi\-node

  Description: \(Required\) The type of cluster you want to assign to your cluster\.
+ NodeType

  Type: String

  Valid values: ds2\.xlarge \| ds2\.8xlarge \| dc1\.large \| dc1\.8xlarge \| dc2\.large \| dc2\.8xlarge \| ra3\.4xlarge \| ra3\.16xlarge

  Description: \(Required\) The type of node you want to assign to your cluster\.
+ NumberOfNodes

  Type: Integer

  Valid values: 2\-100

  Description: \(Optional\) The number of nodes you want to assign to your cluster\. If your cluster is a `single-node` type, do not specify a value for this parameter\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `redshift:DescribeClusters`
+ `redshift:ResizeCluster`

**Document Steps**
+ aws:executeScript \- Modifies the node type and number of nodes for the cluster specified in the `ClusterIdentifer` parameter\.