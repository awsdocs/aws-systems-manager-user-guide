# AWSConfigRemediation\-EnableRDSInstanceDeletionProtection<a name="automation-aws-enable-rds-instance-deletion-protection"></a>

**Description**

The AWSConfigRemediation\-EnableRDSInstanceDeletionProtection runbook enables deletion protection on the Amazon RDS database instance you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableRDSInstanceDeletionProtection)

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
+ DbInstanceResourceId

  Type: String

  Description: \(Required\) The resource identifier for the DB instance you want to enable deletion protection on\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `rds:DescribeDBInstances`
+ `rds:ModifyDBInstance`

**Document Steps**
+ aws:executeAwsApi \- Gathers the DB instance identifier from the DB instance resource identifier\.
+ aws:executeAwsApi \- Enables deletion protection on your DB instance\.
+ aws:assertAwsResourceProperty \- Confirms deletion protection is enabled on the DB instance\.