# AWS\-ConfigureS3BucketVersioning<a name="automation-aws-configures3bucketversioning"></a>

**Description**

Configure versioning for an Amazon Simple Storage Service \(Amazon S3\) bucket\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-ConfigureS3BucketVersioning)

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

  Description: \(Required\) The name of the S3 Bucket whose encryption configuration will be managed\.
+ VersioningState

  Type: String

  Valid values: Enabled \| Suspended

  Default: Enabled

  Description: \(Optional\) Applied to the VersioningConfiguration\.Status\. When set to 'Enabled', this process enables versioning for the objects in the bucket, all objects added to the bucket receive a unique version ID\. When set to `Suspended`, this process disables versioning for the objects in the bucket\. All objects added to the bucket receive the version ID `null`\.