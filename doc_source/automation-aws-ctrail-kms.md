# AWSConfigRemediation\-EnableCloudTrailEncryptionWithKMS<a name="automation-aws-ctrail-kms"></a>

**Description**

The AWSConfigRemediation\-EnableCloudTrailEncryptionWithKMS runbook encrypts an AWS CloudTrail \(CloudTrail\) trail using the AWS Key Management Service \(AWS KMS\) customer managed key you specify\. This runbook should only be used as a baseline to ensure that your CloudTrail trails are encrypted according to minimum recommended security best practices\. We recommend encrypting multiple trails with different KMS keys\. CloudTrail digest files are not encrypted\. If you have previously set the `EnableLogFileValidation` parameter to `true` for the trail, see the "Use server\-side encryption with AWS KMS managed keys" section of the [CloudTrail Preventative Security Best Practices](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/best-practices-security.html#best-practices-security-preventative) topic in the *AWS CloudTrail User Guide* for more information\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableCloudTrailEncryptionWithKMS)

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
+ KMSKeyId

  Type: String

  Description: \(Required\) The ARN, key ID, or the key alias of the of the customer managed key you want to use to encrypt the trail you specify in the `TrailName` parameter\.
+ TrailName

  Type: String

  Description: \(Required\) The ARN or name of the trail you want to update to be encrypted\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `cloudtrail:GetTrail`
+ `cloudtrail:UpdateTrail`

**Document Steps**
+ aws:executeAwsApi \- Enables encryption on the trail you specify in the `TrailName` parameter\.
+ aws:executeAwsApi \- Gathers the ARN for the customer managed key you specify in the `KMSKeyId` parameter\.
+ aws:assertAwsResourceProperty \- Verifies that encryption has been enabled on the CloudTrail trail\.