# AWSConfigRemediation\-CancelKeyDeletion<a name="automation-aws-cancel-key-deletion"></a>

**Description**

The AWSConfigRemediation\-CancelKeyDeletion runbook cancels deletion of the AWS Key Management Service \(AWS KMS\) key that you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-CancelKeyDeletion)

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

  Description: \(Required\) The ID of the key that you want to cancel deletion for\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `kms:CancelKeyDeletion`
+ `kms:DescribeKey`

**Document Steps**
+ aws:executeAwsApi \- Cancels deletion for the key you specify in the `KeyId` parameter\.
+ aws:assertAwsResourceProperty \- Confirms deletion is disabled on your key\.