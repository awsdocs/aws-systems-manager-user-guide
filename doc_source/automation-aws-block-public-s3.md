# AWSConfigRemediation\-ConfigureS3PublicAccessBlock<a name="automation-aws-block-public-s3"></a>

**Description**

The AWSConfigRemediation\-ConfigureS3PublicAccessBlock runbook configures an AWS account's Amazon Simple Storage Service \(Amazon S3\) public access block settings based on the values you specify in the runbook parameters\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-ConfigureS3PublicAccessBlock)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AccountId

  Type: String

  Description: \(Required\) The ID of the AWS account that owns the S3 bucket you are configuring\.
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ BlockPublicAcls

  Type: Boolean

  Default: True

  Description: \(Optional\) If set to `True`, Amazon S3 blocks public access control lists \(ACLs\) for S3 buckets owned by the AWS account you specify in the `AccountId` parameter\.
+ BlockPublicPolicy

  Type: Boolean

  Default: True

  Description: \(Optional\) If set to `True`, Amazon S3 blocks public bucket policies for S3 buckets owned by the AWS account you specify in the `AccountId` parameter\.
+ IgnorePublicAcls

  Type: Boolean

  Default: True

  Description: \(Optional\) If set to `True`, Amazon S3 ignores all public ACLs for S3 buckets owned by the AWS account you specify in the `AccountId` parameter\.
+ RestrictPublicBuckets

  Type: Boolean

  Default: True

  Description: \(Optional\) If set to `True`, Amazon S3 restricts public bucket policies for S3 buckets owned by the AWS account you specify in the `AccountId` parameter\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `s3:GetAccountPublicAccessBlock`
+ `s3:PutAccountPublicAccessBlock`

**Document Steps**
+ aws:executeAwsApi \- Creates or modifies the `PublicAccessBlock` configuration for the AWS account specified in the `AccountId` parameter\.
+ aws:executeScript \- Returns the `PublicAccessBlock` configuration for the AWS account specified in the `AccountId` parameter, and verifies the changes were successfully made based on the values specified in the runbook parameters\.