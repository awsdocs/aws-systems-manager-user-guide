# AWSConfigRemediation\-EncryptSNSTopic<a name="automation-aws-encrypt-sns-topic"></a>

**Description**

The AWSConfigRemediation\-EncryptSNSTopic runbook enables encryption on the Amazon Simple Notification Service \(Amazon SNS\) topic you specify using an AWS Key Management Service \(AWS KMS\) customer managed key\. This runbook should only be used as a baseline to ensure that your Amazon SNS topics are encrypted according to minimum recommended security best practices\. We recommend encrypting multiple topics with different customer managed keys\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EncryptSNSTopic)

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
+ KmsKeyArn

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS KMS customer managed key you want to use to encrypt the Amazon SNS topic\.
+ TopicArn

  Type: String

  Description: \(Required\) The ARN of the Amazon SNS topic you want to encrypt\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `sns:GetTopicAttributes`
+ `sns:SetTopicAttributes`

**Document Steps**
+ aws:executeAwsApi \- Encrypts the Amazon SNS topic you specify in the `TopicArn` parameter\.
+ aws:assertAwsResourceProperty \- Confirms encryption is enabled on the Amazon SNS topic\.