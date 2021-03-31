# AWSConfigRemediation\-EnableELBDeletionProtection<a name="automation-aws-enable-elb-protection"></a>

**Description**

The AWSConfigRemediation\-EnableELBDeletionProtection runbook enables deletion protection for the elastic load balancer \(ELB\) you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableELBDeletionProtection)

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

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the ELB that you want to enable deletion protection on\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `elasticloadbalancing:DescribeLoadBalancerAttributes`
+ `elasticloadbalancing:DescribeLoadBalancers`
+ `elasticloadbalancing:ModifyLoadBalancerAttributes`

**Document Steps**
+ aws:executeScript \- Enables deletion protection on the ELB you specify in the `LoadBalancerArn` parameter\.