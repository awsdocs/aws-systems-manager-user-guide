# AWSConfigRemediation\-CancelKeyDeletion<a name="automation-aws-cancel-key-deletion"></a>

**Description**

The AWSConfigRemediation\-CancelKeyDeletion runbook cancels deletion of the AWS Key Management Service \(AWS KMS\) customer master key \(CMK\) that you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableKeyRotation)

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

  Description: \(Required\) The ID of the CMK that you want to cancel deletion for\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `kms:CancelKeyDeletion`
+ `kms:DescribeKey`

**Document Steps**
+ aws:executeAwsApi \- Cancels deletion for the CMK you specify in the `KeyId` parameter\.
+ aws:assertAwsResourceProperty \- Confirms key deletion is disabled on your CMK\.