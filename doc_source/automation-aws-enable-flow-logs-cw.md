# AWSConfigRemediation\-EnableVPCFlowLogsToCloudWatch<a name="automation-aws-enable-flow-logs-cw"></a>

**Description**

The AWSConfigRemediation\-EnableVPCFlowLogsToCloudWatch runbook replaces an existing Amazon VPC flow log that publishes flow log data to Amazon Simple Storage Service \(Amazon S3\) with a flow log that publishes flow log data to the Amazon CloudWatch Logs \(CloudWatch Logs\) log group you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableVPCFlowLogsToCloudWatch)

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

  Description: \(Required\) The name of the CloudWatch Logs log group you want to publish flow log data to\.
+ DeliverLogsPermissionArn

  Type: String

  Description: \(Required\) The ARN of the AWS Identity and Access Management \(IAM\) role you want to use that provides Amazon Elastic Compute Cloud \(Amazon EC2\) the requisite permissions to publish flow log data to CloudWatch Logs\.
+ FlowLogId

  Type: String

  Description: \(Required\) The ID of the flow log that publishes to Amazon S3 you want to replace\.
+ MaxAggregationInterval

  Type: Integer

  Valid values: 60 \| 600

  Description: \(Optional\) The maximum interval of time, in seconds, during which a flow of packets is captured and aggregated into a flow log record\.
+ TrafficType

  Type: String

  Valid values: ACCEPT \| REJECT \| ALL

  Description: \(Required\) The type of flow log data you want to record and publish\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ec2:CreateFlowLogs`
+ `ec2:DeleteFlowLogs`
+ `ec2:DescribeFlowLogs`

**Document Steps**
+ aws:executeAwsApi \- Gathers details about your VPC from the value you specify in the `FlowLogId` parameter\.
+ aws:executeAwsApi \- Creates a flow log based on the values you specify for the runbook parameters\.
+ aws:assertAwsResourceProperty \- Verifies the newly created flow log publishes to CloudWatch Logs\.
+ aws:executeAwsApi \- Deletes the flow log that publishes to Amazon S3\.
+ aws:executeScript \- Confirms the flow log that published to Amazon S3 was deleted\.