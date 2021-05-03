# AWSConfigRemediation\-EnableVPCFlowLogsToS3Bucket<a name="automation-aws-enable-flow-logs-s3"></a>

**Description**

The AWSConfigRemediation\-EnableVPCFlowLogsToS3Bucket runbook replaces an existing Amazon VPC flow log that publishes flow log data to Amazon CloudWatch Logs \(CloudWatch Logs\) with a flow log that publishes flow log data to the Amazon Simple Storage Service \(Amazon S3\) bucket you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableVPCFlowLogsToS3Bucket)

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
+ DestinationS3BucketArn

  Type: String

  Description: \(Required\) The ARN of the Amazon S3 bucket you want to publish flow log data to\.
+ FlowLogId

  Type: String

  Description: \(Required\) The ID of the flow log that publishes to CloudWatch Logs you want to replace\.
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
+ aws:assertAwsResourceProperty \- Verifies the newly created flow log publishes to Amazon S3\.
+ aws:executeAwsApi \- Deletes the flow log that publishes to CloudWatch Logs\.
+ aws:executeScript \- Confirms the flow log that published to CloudWatch Logs was deleted\.