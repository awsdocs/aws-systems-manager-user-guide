# AWSConfigRemediation\-EnableRDSInstanceBackup<a name="automation-aws-enable-rds-instance-backup"></a>

**Description**

The AWSConfigRemediation\-EnableRDSInstanceBackup runbook enables backups for the Amazon Relational Database Service \(Amazon RDS\) database instance you specify\. This runbook does not support enabling backups for Amazon Aurora database instances\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableRDSInstanceBackup)

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

  Description: \(Optional\) If you specify `true` for this parameter, the modifications in this request and any pending modifications are asynchronously applied as soon as possible, regardless of the `PreferredMaintenanceWindow` setting for the DB instance\.
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ BackupRetentionPeriod

  Type: Integer

  Valid values: 1\-35

  Description: \(Required\) The number of days that backups are retained\.
+ DbiResourceId

  Type: String

  Description: \(Required\) The resource identifier for the DB instance you want to enable backups for\.
+ PreferredBackupWindow

  Type: String

  Description: \(Optional\) The daily time range \(in UTC\) during which backups are created\.

  Constraints:
  + Must be in the format `hh24:mi-hh24:mi`
  + Must be in Coordinated Universal Time \(UTC\)
  + Must not conflict with the preferred maintenance window
  + Must be at least 30 minutes

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `rds:DescribeDBInstances`
+ `rds:ModifyDBInstance`

**Document Steps**
+ aws:executeScript \- Gathers the DB instance identifier from the DB instance resource identifier\. Enables backups for your DB instance\. Confirms backups are enabled on the DB instance\.