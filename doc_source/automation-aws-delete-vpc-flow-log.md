# AWSConfigRemediation\-DeleteVPCFlowLog<a name="automation-aws-delete-vpc-flow-log"></a>

**Description**

The AWSConfigRemediation\-DeleteVPCFlowLog runbook deletes the virtual private cloud \(VPC\) flow log you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DeleteVPCFlowLog)

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
+ FlowLogId

  Type: String

  Description: \(Required\) The ID of the flow log that you want to delete\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ec2:DeleteFlowLogs`
+ `ec2:DescribeFlowLogs`

**Document Steps**
+ aws:executeAwsApi \- Deletes the flow log you specify in the `FlowLogId` parameter\.
+ aws:executeScript \- Verifies the flow log has been deleted\.