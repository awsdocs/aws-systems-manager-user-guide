# AWS\-ConfigureS3BucketLogging<a name="automation-aws-configures3bucketlogging"></a>

**Description**

Enable logging on an Amazon Simple Storage Service \(Amazon S3\) bucket\.

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

  Description: \(Required\) The name of the Amazon S3 Bucket for which you want to configure logging\.
+ GrantedPermission

  Type: String

  Allowed values: FULL\_CONTROL,READ,WRITE

  Description: \(Required\) Logging permissions assigned to the grantee for the bucket\.
+ GranteeEmailAddress

  Type: String

  \(Optional\) Email address of the grantee\.
+ GranteeId

  Type: String

  Description: \(Optional\) The canonical user ID of the grantee\.
+ GranteeType

  Type: String

  Allowed values: CanonicalUser,AmazonCustomerByEmail,Group

  Description: \(Required\) Type of grantee\.
+ GranteeUri

  Type: String

  Description: \(Optional\) URI of the grantee group\.
+ TargetBucket

  Type: String

  Description: \(Required\) Specifies the bucket where you want Amazon S3 to store server access logs\. You can have your logs delivered to any bucket that you own\. You can also configure multiple buckets to deliver their logs to the same target bucket\. In this case you should choose a different TargetPrefix for each source bucket so that the delivered log files can be distinguished by key\.
+ TargetPrefix

  Type: String

  Default: /

  Description: \(Optional\) Specifies a prefix for the keys under which the log files will be stored\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-ConfigureS3BucketLogging --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```