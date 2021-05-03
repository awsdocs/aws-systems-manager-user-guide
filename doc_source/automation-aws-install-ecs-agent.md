# AWS\-InstallECSContainerAgent<a name="automation-aws-install-ecs-agent"></a>

**Description**

The AWS\-InstallECSContainerAgent runbook installs the Amazon Elastic Container Service \(Amazon ECS\) agent on the Amazon Elastic Compute Cloud \(Amazon EC2\) instance you specify\. This runbook only supports Amazon Linux and Amazon Linux 2 instances\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-InstallECSContainerAgent)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this runbook\.
+ InstanceIds

  Type: StringList

  Description: \(Required\) The IDs of the Amazon EC2 instances you want to install the Amazon ECS agent on\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ssm:GetCommandInvocation`
+ `ec2:DescribeImages`
+ `ec2:DescribeInstanceAttribute`
+ `ec2:DescribeInstances`

**Document Steps**

aws:executeScript \- Installs the Amazon ECS agent on the Amazon EC2 instances you specify in the `InstanceIds` parameter\.

**Outputs**

InstallAmazonECSAgent\.SuccessfulInstances \- The ID of the instance where installation of the Amazon ECS agent succeeded\.

InstallAmazonECSAgent\.FailedInstances \- The ID of the instance where installation of the Amazon ECS agent failed\.

InstallAmazonECSAgent\.InProgressInstances \- The ID of the instance where installation of the Amazon ECS agent is in progress\.