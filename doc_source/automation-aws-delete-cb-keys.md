# AWSConfigRemediation\-DeleteAccessKeysFromCodeBuildProject<a name="automation-aws-delete-cb-keys"></a>

**Description**

The AWSConfigRemediation\-DeleteAccessKeysFromCodeBuildProject runbook deletes the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` environment variables from the AWS CodeBuild \(CodeBuild\) project you specify\. AWS Config must be enabled in the AWS Region where you run this automation\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DeleteAccessKeysFromCodeBuildProject)

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
+ ResourceId

  Type: String

  Description: \(Required\) The ID of the CodeBuild project whose access key environment variables you want to delete\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `config:GetResourceConfigHistory`
+ `codebuild:BatchGetProjects`
+ `codebuild:UpdateProject`

**Document Steps**
+ aws:executeScript \- Deletes the access key environment variables for the CodeBuild project specified in the `ResourceId` parameter\.