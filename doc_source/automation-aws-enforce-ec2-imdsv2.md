# AWSConfigRemediation\-EnforceEC2InstanceIMDSv2<a name="automation-aws-enforce-ec2-imdsv2"></a>

**Description**

The AWSConfigRemediation\-EnforceEC2InstanceIMDSv2 runbook requires the Amazon Elastic Compute Cloud \(Amazon EC2\) instance you specify to use Instance Metadata Service Version 2 \(IMDSv2\)\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnforceEC2InstanceIMDSv2)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ InstanceId

  Type: String

  Description: \(Required\) The ID of the Amazon EC2 instance you want to require to use IMDSv2\.
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ec2:DescribeInstances`
+ `ec2:ModifyInstanceMetadataOptions`

**Document Steps**
+ aws:executeScript \- Sets the `HttpTokens` option to `required` on the Amazon EC2 instance you specify in the `InstanceId` parameter\.
+ aws:assertAwsResourceProperty \- Verifies IMDSv2 is required on the Amazon EC2 instance\.