# AWSConfigRemediation\-EnableRedshiftClusterAuditLogging<a name="automation-aws-enable-redshift-audit"></a>

**Description**

The AWSConfigRemediation\-EnableRedshiftClusterAuditLogging runbook enables audit logging for the Amazon Redshift cluster you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableRedshiftClusterAuditLogging)

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
+ BucketName

  Type: String

  Description: \(Required\) The name of the Amazon Simple Storage Service \(Amazon S3\) bucket you want to upload logs to\.
+ ClusterIdentifer

  Type: String

  Description: \(Required\) The unique identifier of the cluster you want to enable audit logging on\.
+ S3KeyPrefix

  Type: String

  Description: \(Optional\) The Amazon S3 key prefix \(subfolder\) you want to upload logs to\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `redshift:DescribeLoggingStatus`
+ `redshift:EnableLogging`
+ `s3:GetBucketAcl`
+ `s3:PutObject`

**Document Steps**
+ aws:branch \- Branches based on whether a value was specified for the `S3KeyPrefix` parameter\.
+ aws:executeAwsApi \- Enables audit logging on the cluster specified in the `ClusterIdentifer` parameter\.
+ aws:assertAwsResourceProperty \- Verifies audit logging was enabled on the cluster\.