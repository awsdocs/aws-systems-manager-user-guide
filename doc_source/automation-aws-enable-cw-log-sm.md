# AWSConfigRemediation\-EnableCWLoggingForSessionManager<a name="automation-aws-enable-cw-log-sm"></a>

**Description**

The AWSConfigRemediation\-EnableCWLoggingForSessionManager runbook enables AWS Systems Manager Session Manager \(Session Manager\) sessions to store output logs to an Amazon CloudWatch \(CloudWatch\) log group\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableRDSInstanceBackup)

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
+ DestinationLogGroup

  Type: String

  Description: \(Required\) The name of the CloudWatch log group\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ssm:GetDocument`
+ `ssm:UpdateDocument`
+ `ssm:CreateDocument`
+ `ssm:UpdateDefaultDocumentVersion`
+ `ssm:DescribeDocument`

**Document Steps**
+ aws:executeScript \- Accepts the CloudWatch log group to update the document which stores Session Manager session output logs preferences, or creates one if it doesn't exist\.