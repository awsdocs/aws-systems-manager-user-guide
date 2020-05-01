# AWS\-ASGExitStandby<a name="automation-aws-asgexitstandby"></a>

**Description**

Change the standby state of an EC2 instance in an Auto Scaling group\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-ASGExitStandby)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ InstanceId

  Type: String

  Description: \(Required\) ID of an EC2 instance for which you want to change the standby state within an Auto Scaling group\.
+ LambdaRoleArn

  Type: String

  Description: \(Optional\) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf\. If not specified a transient role will be created to run the Lambda function\.