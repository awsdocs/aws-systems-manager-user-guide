# AWSConfigRemediation\-DeleteUnusedENI<a name="automation-aws-delete-eni"></a>

**Description**

The AWSConfigRemediation\-DeleteUnusedENI runbook deletes an elastic network interface \(ENI\) that has an attachment status of `detached`\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DeleteUnusedENI)

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
+ NetworkInterfaceId

  Type: String

  Description: \(Required\) The ID of the ENI that you want to delete\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ec2:DeleteNetworkInterface`
+ `ec2:DescribeNetworkInterfaces `

**Document Steps**
+ aws:executeAwsApi \- Deletes the ENI you specify in the `NetworkInterfaceId` parameter\.
+ aws:executeScript \- Verifies the ENI has been deleted\.