# AWS\-DisableS3BucketPublicReadWrite<a name="automation-aws-disables3bucketpublicreadwrite"></a>

**Description**

Use Amazon Simple Storage Service \(Amazon S3\) `Block Public Access` to disable read and write access for a public Amazon S3 bucket\. For more information, see [Using Amazon S3 Block Public Access](https://docs.aws.amazon.com/AmazonS3/latest/dev/access-control-block-public-access.html) in the *Amazon Simple Storage Service Developer Guide*\. 

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ S3BucketName

  Type: String

  Description: \(Required\) Amazon S3 bucket on which you want to restrict access\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-DisableS3BucketPublicReadWrite --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

aws:executeAwsApi

**Outputs**

None