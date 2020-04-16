# AWS\-EnableCloudTrail<a name="automation-aws-enablecloudtrail"></a>

**Description**

Create an AWS CloudTrail trail and configure logging to an S3 bucket\.

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

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-EnableCloudTrail --parameters TrailName=TrailName,S3BucketName=s3bucketname,AutomationAssumeRole=arn:aws:iam::123456789012:role/AutomationRole
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```