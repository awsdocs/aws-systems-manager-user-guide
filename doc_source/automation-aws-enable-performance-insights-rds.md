# AWSConfigRemediation\-EnablePerformanceInsightsOnRDSInstance<a name="automation-aws-enable-performance-insights-rds"></a>

**Description**

The AWSConfigRemediation\-EnablePerformanceInsightsOnRDSInstance runbook enables Performance Insights on the Amazon RDS DB instance you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnablePerformanceInsightsOnRDSInstance)

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

  Description: \(Required\) The resource identifier for the DB instance you want to enable Performance Insights on\.
+ PerformanceInsightsKMSKeyId

  Type: String

  Default: alias/aws/rds

  Description: \(Optional\) The Amazon Resource Name \(ARN\), key ID, or the key alias of the AWS Key Management Service \(AWS KMS\) customer managed key you want Performance Insights to use to encrypt all potentially sensitive data\. If you enter the key alias for this parameter, prefix the value with **alias/**\. If you do not specify a value for this parameter, the AWS managed key is used\.
+ PerformanceInsightsRetentionPeriod

  Type: Integer

  Valid values: 7, 731

  Default: 7

  Description: \(Optional\) The number of days you want to retain Performance Insights data\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `kms:CreateGrant`
+ `kms:DescribeKey`
+ `rds:DescribeDBInstances`
+ `rds:ModifyDBInstance`

**Document Steps**
+ aws:executeAwsApi \- Gathers the DB instance identifier from the DB instance resource identifier\.
+ aws:assertAwsResourceProperty \- Confirms the DB instance status is `available`\.
+ aws:executeAwsApi \- Gathers the ARN of the AWS KMS customer managed key specified in the `PerformanceInsightsKMSKeyId` parameter\.
+ aws:branch \- Checks whether a value is already assigned to the `PerformanceInsightsKMSKeyId` property of the DB instance\.
+ aws:executeAwsApi \- Enables Performance Insights on the DB instance you specify in the `DbiResourceId` parameter\.
+ aws:assertAwsResourceProperty \- Confirms the value specified for the `PerformanceInsightsKMSKeyId` parameter was used to enable encryption for Performance Insights on the DB instance\.
+ aws:assertAwsResourceProperty \- Confirms Performance Insights is enabled on the DB instance\.