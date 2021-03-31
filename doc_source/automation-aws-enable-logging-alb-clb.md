# AWSConfigRemediation\-EnableLoggingForALBAndCLB<a name="automation-aws-enable-logging-alb-clb"></a>

**Description**

The AWSConfigRemediation\-EnableLoggingForALBAndCLB runbook enables logging for the specified AWS Application Load Balancer or a Classic Load Balancer \(CLB\)\.

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
+ LoadBalancerId

  Type: String

  Description: \(Required\) The Classic Load Balancer name or the Application Load Balancer ARN\.
+ S3BucketName

  Type: String

  Description: \(Required\) The Amazon S3 bucket name\.
+ S3BucketPrefix

  Type: String

  Description: \(Optional\) The logical hierarchy you created for your Amazon Simple Storage Service \(Amazon S3\) bucket, for example `my-bucket-prefix/prod`\. If the prefix is not provided, the log is placed at the root level of the bucket\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `elasticloadbalancing:DescribeLoadBalancerAttributes`
+ `elasticloadbalancing:ModifyLoadBalancerAttributes`

**Document Steps**
+ aws:executeScript \- Enables and verifies the logging for the Classic Load Balancer or the Application Load Balancer\.