# AWSConfigRemediation\-EnableMultiAZOnRDSInstance<a name="automation-aws-multi-az-rds"></a>

**Description**

The AWSConfigRemediation\-EnableMultiAZOnRDSInstance runbook changes your Amazon Relational Database Service \(Amazon RDS\) database \(DB\) instance to a Multi\-AZ deployment\. Changing this setting doesn't result in an outage\. The change is applied during the next maintenance window unless you set the `ApplyImmediately` parameter to `true`\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableMultiAZOnRDSInstance)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ ApplyImmediately

  Type: Boolean

  Default: false

  Description: \(Optional\) If you specify `true` for this parameter, the modifications in this request and any pending modifications are asynchronously applied as soon as possible, regardless of the `PreferredMaintenanceWindow` setting for the DB instance\.
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ DbiResourceId

  Type: String

  Description: \(Required\) The AWS Region\-unique, immutable identifier for the DB instance to enable the `MultiAZ` setting\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `rds:DescribeDBInstances`
+ `rds:ModifyDBInstance`
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`

**Document Steps**
+ aws:executeAwsApi \- Retrieves the DB instance name using the value provided in the `DBInstanceId` parameter\.
+ aws:executeAwsApi \- Verifies the `DBInstanceStatus` is `available`\.
+ aws:branch \- Checks whether the `MultiAZ` is already set to `true` on the DB instance you specify in the `DbiResourceId` parameter\.
+ aws:executeAwsApi \- Changes the `MultiAZ` setting to `true` on the DB instance you specify in the `DbiResourceId` parameter\.
+ aws:assertAwsResourceProperty \- Verifies the `MultiAZ` is set to `true` on the DB instance you specify in the `DbiResourceId` parameter\.