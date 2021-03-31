# AWSConfigRemediation\-CreateCloudTrailMultiRegionTrail<a name="automation-aws-create-ct-mr"></a>

**Description**

The AWSConfigRemediation\-CreateCloudTrailMultiRegionTrail runbook creates an AWS CloudTrail \(CloudTrail\) trail that delivers log files from multiple AWS Regions to the Amazon Simple Storage Service \(Amazon S3\) bucket of your choice\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableRDSInstanceBackup)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ BucketName

  Type: String

  Description: \(Required\) The name of the Amazon S3 bucket you want to upload logs to\.
+ KeyPrefix

  Type: String

  Description: \(Optional\) The Amazon S3 key prefix that comes after the name of the bucket you designated for log file delivery\.
+ TrailName

  Type: String

  Description: \(Required\) The name of the CloudTrail trail to be created\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `cloudtrail:CreateTrail`
+ `cloudtrail:StartLogging`
+ `cloudtrail:GetTrail`
+ `s3:PutObject`
+ `s3:GetBucketAcl`
+ `s3:PutBucketLogging`
+ `s3:ListBucket`

**Document Steps**
+ aws:executeAwsApi \- Accepts the trail name and the Amazon S3 bucket name as input and creates a CloudTrail trail\.
+ aws:executeAwsApi \- Enables logging on the created trail and starts log delivery to the Amazon S3 bucket you specified\.
+ aws:assertAwsResourceProperty \- Verifies that the CloudTrail trail has been created\.