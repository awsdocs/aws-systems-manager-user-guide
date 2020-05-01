# AWS\-SetupManagedRoleOnEC2Instance<a name="automation-aws-setupmanagedroleonec2instance"></a>

**Description**

Configure an instance with the SSMRoleForManagedInstance managed IAM role for Systems Manager access\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-SetupManagedRoleOnEC2Instance)

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

  Description: \(Required\) ID of the EC2 instance to configure
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf\. If not specified a transient role will be created to run the Lambda function\.
+ RoleName

  Type: String

  Default: SSMRoleForManagedInstance

  Description: \(Optional\) The name of the IAM role for the EC2 instance\. If this role does not exist, it will be created\. When specifying this value, verify that the role contains the **AmazonSSMManagedInstanceCore** Managed Policy\.