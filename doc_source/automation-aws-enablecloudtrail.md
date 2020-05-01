# AWS\-EnableCloudTrail<a name="automation-aws-enablecloudtrail"></a>

**Description**

Create an AWS CloudTrail trail and configure logging to an S3 bucket\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-EnableCloudTrail)

**Document Type**

Automation

**Owner**

Amazon

**Platform\(s\)**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ S3BucketName

  Type: String

  Description: \(Required\) Name of the S3 bucket designated for publishing log files\.
**Note**  
The S3 bucket must exist and the bucket policy must grant CloudTrail permission to write to it\. For information, see [Amazon S3 Bucket Policy for CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/create-s3-bucket-policy-for-cloudtrail.html)\.
+ TrailName

  Type: String

  Description: \(Required\) The name of the new trail\.