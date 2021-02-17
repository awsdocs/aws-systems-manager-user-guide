# AWSConfigRemediation\-EnableCLBCrossZoneLoadBalancing<a name="automation-aws-enable-clb-crosszone"></a>

**Description**

The AWSConfigRemediation\-EnableCLBCrossZoneLoadBalancing runbook enables cross\-zone load balancing for the Classic Load Balancer \(CLB\) you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableCLBCrossZoneLoadBalancing)

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
+ LoadBalancerName

  Type: String

  Description: \(Required\) The name of the CLB that you want to enable cross\-zone load balancing on\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `elb:DescribeLoadBalancerAttributes`
+ `elb:ModifyLoadBalancerAttributes`

**Document Steps**
+ aws:executeAwsApi \- Enables cross\-zone load balancing for the CLB you specify in the `LoadBalancerName` parameter\.
+ aws:assertAwsResourceProperty \- Verifies cross\-zone load balancing has been enabled on the CLB\.