# AWSConfigRemediation\-RemoveVPCDefaultSecurityGroupRules<a name="automation-aws-remove-default-secg-rules"></a>

**Description**

The AWSConfigRemediation\-RemoveVPCDefaultSecurityGroupRules runbook removes all rules from the default security group of the virtual private cloud \(VPC\) you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-RemoveVPCDefaultSecurityGroupRules)

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
+ GroupId

  Type: String

  Description: \(Required\) The ID of the security group that you want to remove all rules from\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ec2:DescribeSecurityGroups`
+ `ec2:RevokeSecurityGroupEgress`
+ `ec2:RevokeSecurityGroupIngress`

**Document Steps**
+ aws:assertAwsResourceProperty \- Confirms the security group you specified in the `GroupId` parameter is named default\.
+ aws:executeScript \- Removes all rules from the security group you specified in the `GroupId` parameter\.