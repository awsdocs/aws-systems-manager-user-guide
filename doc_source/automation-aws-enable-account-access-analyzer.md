# AWSConfigRemediation\-EnableAccountAccessAnalyzer<a name="automation-aws-enable-account-access-analyzer"></a>

**Description**

The AWSConfigRemediation\-EnableAccountAccessAnalyzer runbook creates an AWS Identity and Access Management \(IAM\) Access Analyzer in your AWS account\. For information about Access Analyzer, see [Using AWS IAM Access Analyzer](https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) in the *IAM User Guide*\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableAccountAccessAnalyzer)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AnalyzerName

  Type: String

  Description: \(Required\) The name of the analyzer to create\.
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `access-analyzer:CreateAnalyzer`
+ `access-analyzer:GetAnalyzer`

**Document Steps**
+ aws:executeAwsApi \- Creates an access analyzer for your account\.
+ aws:waitForAwsResourceProperty \- Waits for the status of the access analyzer to be `ACTIVE`\.
+ aws:assertAwsResourceProperty \- Confirms the status of the access analyzer is `ACTIVE`\.