# AWSConfigRemediation\-EnableNLBCrossZoneLoadBalancing<a name="automation-aws-enable-nlb-crosszone"></a>

**Description**

The AWSConfigRemediation\-EnableNLBCrossZoneLoadBalancing runbook enables cross zone load balancing for the network load balancer \(NLB\) you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableNLBCrossZoneLoadBalancing)

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
+ LoadBalancerArn

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the NLB that you want to enable cross zone load balancing on\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `elbv2:DescribeLoadBalancerAttributes`
+ `elbv2:ModifyLoadBalancerAttributes`

**Document Steps**
+ aws:executeAwsApi \- Enables cross zone load balancing for the NLB you specify in the `LoadBalancerArn` parameter\.
+ aws:executeScript \- Verifies cross zone load balancing has been enabled on the NLB\.