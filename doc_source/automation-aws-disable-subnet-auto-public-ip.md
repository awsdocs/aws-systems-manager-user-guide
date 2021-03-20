# AWSConfigRemediation\-DisableSubnetAutoAssignPublicIP<a name="automation-aws-disable-subnet-auto-public-ip"></a>

**Description**

The AWSConfigRemediation\-DisableSubnetAutoAssignPublicIP runbook disables the IPv4 public addressing attribute for the subnet you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DisableSubnetAutoAssignPublicIP)

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
+ SubnetId

  Type: String

  Description: \(Required\) The ID of the subnet that you want to disable the auto\-assign public IPv4 address attribute on\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ec2:DescribeSubnets`
+ `ec2:ModifySubnetAttribute`

**Document Steps**
+ aws:executeAwsApi \- Disables the auto\-assign public IPv4 address attribute for the subnet you specified in the `SubnetId` parameter\.
+ aws:assertAwsResourceProperty \- Verifies the attribute has been disabled\.