# AWSConfigRemediation\-DeleteEgressOnlyInternetGateway<a name="automation-aws-delete-egress-igw"></a>

**Description**

The AWSConfigRemediation\-DeleteEgressOnlyInternetGateway runbook deletes the egress\-only internet gateway you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DeleteEgressOnlyInternetGateway)

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
+ EgressOnlyInternetGatewayId

  Type: String

  Description: \(Required\) The ID of the egress\-only internet gateway that you want to delete\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ec2:DeleteEgressOnlyInternetGateway`
+ `ec2:DescribeEgressOnlyInternetGateways`

**Document Steps**
+ aws:executeScript \- Deletes the egress\-only internet gateway specified in the `EgressOnlyInternetGatewayId` parameter\.
+ aws:executeScript \- Verifies the egress\-only internet gateway has been deleted\.