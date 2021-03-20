# AWSConfigRemediation\-DeleteRDSCluster<a name="automation-aws-delete-rds-cluster"></a>

**Description**

The AWSConfigRemediation\-DeleteRDSCluster runbook deletes the Amazon Relational Database Service \(Amazon RDS\) cluster you specify\. AWS Config must be enabled in the AWS Region where you run this automation\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DeleteRDSCluster)

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
+ DBClusterId

  Type: String

  Description: \(Required\) The resource identifier for the DB cluster you want to enable deletion protection on\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `config:GetResourceConfigHistory`
+ `rds:DeleteDBCluster`
+ `rds:DeleteDBInstance`
+ `rds:DescribeDBClusters`

**Document Steps**
+ aws:executeScript \- Deletes the DB cluster you specify in the `DBClusterId` parameter\.