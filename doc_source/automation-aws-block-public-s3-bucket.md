# AWSConfigRemediation\-ConfigureS3BucketPublicAccessBlock<a name="automation-aws-block-public-s3-bucket"></a>

**Description**

The AWSConfigRemediation\-ConfigureS3BucketPublicAccessBlock runbook configures the Amazon Simple Storage Service \(Amazon S3\) public access block settings for an Amazon S3 bucket based on the values you specify in the runbook parameters\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-ConfigureS3BucketPublicAccessBlock)

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
+ BlockPublicAcls

  Type: Boolean

  Default: True

  Description: \(Optional\) If set to `True`, Amazon S3 blocks public access control lists \(ACLs\) for the S3 bucket, and objects stored in the S3 bucket you specify in the `BucketName` parameter\.
+ BlockPublicPolicy

  Type: Boolean

  Default: True

  Description: \(Optional\) If set to `True`, Amazon S3 blocks public bucket policies for the S3 bucket you specify in the `BucketName` parameter\.
+ BucketName

  Type: String

  Description: \(Required\) The name of the S3 bucket you want to configure\.
+ IgnorePublicAcls

  Type: Boolean

  Default: True

  Description: \(Optional\) If set to `True`, Amazon S3 ignores all public ACLs for the S3 bucket you specify in the `BucketName` parameter\.
+ RestrictPublicBuckets

  Type: Boolean

  Default: True

  Description: \(Optional\) If set to `True`, Amazon S3 restricts public bucket policies for the S3 bucket you specify in the `BucketName` parameter\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `s3:GetAccountPublicAccessBlock`
+ `s3:PutAccountPublicAccessBlock`

**Document Steps**
+ aws:executeAwsApi \- Creates or modifies the `PublicAccessBlock` configuration for the S3 bucket specified in the `BucketName` parameter\.
+ aws:executeScript \- Returns the `PublicAccessBlock` configuration for the S3 bucket specified in the `BucketName` parameter, and verifies the changes were successfully made based on the values specified in the runbook parameters\.