# AWSConfigRemediation\-RestrictBucketSSLRequestsOnly<a name="automation-aws-s3-deny-http"></a>

**Description**

The AWSConfigRemediation\-RestrictBucketSSLRequestsOnly runbook creates an Amazon Simple Storage Service \(Amazon S3\) bucket policy statement that explicitly denies HTTP requests to the Amazon S3 bucket you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-RestrictBucketSSLRequestsOnly)

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

  Description: \(Required\) The name of the S3 bucket that you want to deny HTTP requests\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `s3:DeleteBucketPolicy`
+ `s3:GetBucketPolicy`
+ `s3:PutBucketPolicy`

**Document Steps**
+ aws:executeScript \- Creates a bucket policy for the S3 bucket specified in the `BucketName` parameter that explicitly denies HTTP requests\.