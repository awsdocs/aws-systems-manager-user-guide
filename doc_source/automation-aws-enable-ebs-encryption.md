# AWSConfigRemediation\-EnableEbsEncryptionByDefault<a name="automation-aws-enable-ebs-encryption"></a>

**Description**

The AWSConfigRemediation\-EnableEbsEncryptionByDefault runbook enables encryption on all new Amazon Elastic Block Store \(Amazon EBS\) volumes in the AWS account and AWS Region where you run the automation\. Volumes that were created before you run the automation are not encrypted\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableEbsEncryptionByDefault)

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

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ec2:EnableEbsEncryptionByDefault`
+ `ssm:ExecuteAutomation`
+ `ssm:GetAutomationExecution`

**Document Steps**
+ aws:executeAwsApi \- Enables the default Amazon EBS encryption setting in the current account and Region\.
+ aws:assertAwsResourceProperty \- Verifies that the default Amazon EBS encryption setting has been enabled\.