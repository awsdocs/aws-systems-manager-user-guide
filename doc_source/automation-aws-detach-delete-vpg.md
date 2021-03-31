# AWSConfigRemediation\-DetachAndDeleteVirtualPrivateGateway<a name="automation-aws-detach-delete-vpg"></a>

**Description**

The AWSConfigRemediation\-DetachAndDeleteVirtualPrivateGateway runbook detaches and deletes a given Amazon Elastic Compute Cloud \(Amazon EC2\) virtual private gateway attached to a virtual private cloud \(VPC\) created with Amazon Virtual Private Cloud \(Amazon VPC\)\.

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
+ VpnGatewayId

  Type: String

  Description: \(Required\) The ID of the virtual private gateway to be deleted\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ec2:DeleteVpnGateway`
+ `ec2:DetachVpnGateway`
+ `ec2:DescribeVpnGateways`

**Document Steps**
+ aws:waitForAwsResourceProperty \- Accepts the ID of the virtual private gateway and waits until the virtual private gateway's state property changes to `available` or times out\.
+ aws:executeAwsApi \- Retrieves a specified virtual private gateway configuration\.
+ aws:branch \- Branches based on the VpcAttachments\.state parameter value\.
+ aws:waitForAwsResourceProperty \- Accepts the ID of the virtual private gateway and waits until the virtual private gateway's VpcAttachments\.state's property changes to `attached` or times out\.
+ aws:executeAwsApi \- Accepts the ID of the virtual private gateway and the ID of the Amazon VPC as input, and detaches the virtual private gateway from the Amazon VPC\.
+ aws:waitForAwsResourceProperty \- Accepts the ID of the virtual private gateway and waits until the virtual private gateway's VpcAttachments\.state's property changes to `detached` or times out\.
+ aws:executeAwsApi \- Accepts the ID of the virtual private gateway as input and deletes it\.
+ aws:waitForAwsResourceProperty \- Accepts the ID of the virtual private gateway as input and verifies its deletion\.