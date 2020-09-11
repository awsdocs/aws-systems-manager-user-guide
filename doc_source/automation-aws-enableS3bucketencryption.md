# AWS\-EnableS3BucketEncryption<a name="automation-aws-enableS3bucketencryption"></a>

**Description**

Enable encryption for an Amazon Simple Storage Service \(Amazon S3\) bucket \(encrypt the contents of the bucket\)\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-EnableS3BucketEncryption)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ BucketName

  Type: String

  Description: \(Required\) The name of the S3 bucket where you want to encrypt the contents\.
+ SSEAlgorithm

  Type: String

  Default: AES256

  Description: \(Optional\) Server\-side encryption algorithm to use for the default encryption\.