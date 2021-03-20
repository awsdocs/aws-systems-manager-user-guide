# AWSConfigRemediation\-ConfigureCodeBuildProjectWithKMSCMK<a name="automation-aws-codebuild-cmk"></a>

**Description**

The AWSConfigRemediation\-ConfigureCodeBuildProjectWithKMSCMK runbook encrypts an AWS CodeBuild \(CodeBuild\) project's build artifacts using the AWS Key Management Service \(AWS KMS\) customer master key \(CMK\) you specify\. AWS Config must be enabled in the AWS Region where you run this automation\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-ConfigureCodeBuildProjectWithKMSCMK)

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

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the CMK you want to use to encrypt the CodeBuild project you specify in the `ProjectId` parameter\.
+ ProjectId

  Type: String

  Description: \(Required\) The ID of the CodeBuild project whose build artifacts you want to encrypt\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `codebuild:BatchGetProjects`
+ `codebuild:UpdateProject`
+ `config:GetResourceConfigHistory`

**Document Steps**
+ aws:executeAwsApi \- Gathers the CodeBuild project name from the project ID\.
+ aws:executeAwsApi \- Enables encryption on the CodeBuild project you specify in the `ProjectId` parameter\.
+ aws:assertAwsResourceProperty \- Verifies that encryption has been enabled on the CodeBuild project\.

**Outputs**

UpdateLambdaConfig\.UpdateFunctionConfigurationResponse \- Response from the `UpdateFunctionConfiguration` API call\.