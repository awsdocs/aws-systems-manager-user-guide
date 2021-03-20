# AWSConfigRemediation\-EnableMinorVersionUpgradeOnRDS<a name="automation-aws-enable-rds-minor-version"></a>

**Description**

The AWSConfigRemediation\-EnableMinorVersionUpgradeOnRDS runbook enables the `AutoMinorVersionUpgrade` setting on the Amazon RDS database instance you specify\. Enabling this setting means that minor version upgrades are applied automatically to the DB instance during the maintenance window\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableMinorVersionUpgradeOnRDS)

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
+ DbiResourceId

  Type: String

  Description: \(Required\) The resource identifier for the DB instance you want to the `AutoMinorVersionUpgrade` setting on\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `rds:DescribeDBInstances`
+ `rds:ModifyDBInstance`

**Document Steps**
+ aws:executeAwsApi \- Gathers the DB instance identifier from the DB instance resource identifier\.
+ aws:assertAwsResourceProperty \- Confirms the DB Instance is in an `AVAILABLE` state\.
+ aws:executeAwsApi \- Enables the `AutoMinorVersionUpgrade` setting on your DB instance\.
+ aws:executeScript \- Confirms that the `AutoMinorVersionUpgrade` setting is enabled on your DB instance\.