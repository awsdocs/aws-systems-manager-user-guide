# AWS\-EnableS3BucketEncryption<a name="automation-aws-enableS3bucketencryption"></a>

**Description**

Enable encryption for an Amazon Simple Storage Service \(Amazon S3\) bucket \(encrypt the contents of the bucket\)\.

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
+ BucketName

  Type: String

  Description: \(Required\) The name of the Amazon S3 bucket where you want to encrypt the contents\.
+ SSEAlgorithm

  Type: String

  Default: AES256

  Description: \(Optional\) Server\-side encryption algorithm to use for the default encryption\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-EnableS3BucketEncryption --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```