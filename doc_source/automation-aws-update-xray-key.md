# AWSConfigRemediation\-UpdateXRayKMSKey<a name="automation-aws-update-xray-key"></a>

**Description**

The AWSConfigRemediation\-UpdateXRayKMSKey runbook enables encryption on your AWS X\-Ray data using an AWS Key Management Service \(AWS KMS\) customer master key \(CMK\)\. This runbook should only be used as a baseline to ensure that your AWS X\-Ray data is encrypted according to minimum recommended security best practices\. We recommend encrypting multiple sets of data with different CMKs\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-UpdateXRayKMSKey)

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
+ KeyId

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\), key ID, or the key alias of the AWS Key Management Service \(AWS KMS\) customer master key \(CMK\) you want AWS X\-Ray to use to encrypt data\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `kms:DescribeKey`
+ `xray:GetEncryptionConfig`
+ `xray:PutEncryptionConfig`

**Document Steps**
+ aws:executeAwsApi \- Enables encryption on your X\-Ray data using the CMK you specify in the `KeyId` parameter\.
+ aws:waitForAwsResourceProperty \- Waits for the encryption configuration status of your X\-Ray to be `ACTIVE`\.
+ aws:executeAwsApi \- Gathers the ARN of the CMK you specify in the `KeyId` parameter\.
+ aws:assertAwsResourceProperty \- Verifies encryption has been enabled on your X\-Ray\.