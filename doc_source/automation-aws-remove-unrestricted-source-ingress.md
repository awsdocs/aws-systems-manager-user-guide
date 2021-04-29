# AWSConfigRemediation\-RemoveUnrestrictedSourceIngressRules<a name="automation-aws-remove-unrestricted-source-ingress"></a>

**Description**

The AWSConfigRemediation\-RemoveUnrestrictedSourceIngressRules runbook removes all ingress rules from the security group you specify that allow traffic from all source addresses\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-RemoveUnrestrictedSourceIngressRules)

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
+ SecurityGroupId

  Type: String

  Description: \(Required\) The ID of the security group that you want to remove ingress rules that allow traffic from all source addresses from\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ec2:DescribeSecurityGroups`
+ `ec2:RevokeSecurityGroupIngress`

**Document Steps**
+ aws:executeScript \- Removes all ingress rules that allow traffic from all source addresses from the security group you specified in the `SecurityGroupId` parameter\.