# AWS\-ConfigureS3BucketVersioning<a name="automation-aws-configures3bucketversioning"></a>

**Description**

Configure versioning for an Amazon Simple Storage Service \(Amazon S3\) bucket\.

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

  Description: \(Required\) The name of the S3 Bucket whose encryption configuration will be managed\.
+ VersioningState

  Type: String

  Allowed values: Enabled,Suspended

  Default: Enabled

  Description: \(Optional\) Applied to the VersioningConfiguration\.Status\. When set to 'Enabled', this process enables versioning for the objects in the bucket, all objects added to the bucket receive a unique version ID\. When set to 'Suspended', this process disables versioning for the objects in the bucket\. All objects added to the bucket receive the version ID `null`\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-ConfigureS3BucketVersioning --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```