# AWS\-DisableS3BucketPublicReadWrite<a name="automation-aws-disables3bucketpublicreadwrite"></a>

**Description**

Use Amazon Simple Storage Service \(Amazon S3\) `Block Public Access` to disable read and write access for a public S3 bucket\. For more information, see [Using Amazon S3 Block Public Access](https://docs.aws.amazon.com/AmazonS3/latest/dev/access-control-block-public-access.html) in the *Amazon Simple Storage Service Developer Guide*\. 

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-DisableS3BucketPublicReadWrite)

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
+ S3BucketName

  Type: String

  Description: \(Required\) S3 bucket on which you want to restrict access\.