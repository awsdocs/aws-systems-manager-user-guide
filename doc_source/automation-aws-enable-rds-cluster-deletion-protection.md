# AWSConfigRemediation\-EnableRDSClusterDeletionProtection<a name="automation-aws-enable-rds-cluster-deletion-protection"></a>

**Description**

The AWSConfigRemediation\-EnableRDSClusterDeletionProtection runbook enables deletion protection on the Amazon Relational Database Service \(Amazon RDS\) cluster you specify\. AWS Config must be enabled in the AWS Region where you run this automation\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableRDSClusterDeletionProtection)

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
+  ClusterId

  Type: String

  Description: \(Required\) The resource identifier for the DB cluster you want to enable deletion protection on\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `config:GetResourceConfigHistory`
+ `rds:DescribeDBClusters`
+ `rds:ModifyDBCluster`

**Document Steps**
+ aws:executeAwsApi \- Gathers the DB cluster name from the DB cluster resource identifier\.
+ aws:assertAwsResourceProperty \- Verifies the DB cluster status is `available`\.
+ aws:executeAwsApi \- Enables deletion protection on the DB cluster you specify in the `ClusterId` parameter\.
+ aws:assertAwsResourceProperty \- Verifies deletion protection has been enabled on the DB cluster\.