# AWS\-ConfigureCloudWatchOnEC2Instance<a name="automation-aws-configurecloudwatchonec2instance"></a>

**Description**

Enable or disable Amazon CloudWatch monitoring on managed instances\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-ConfigureCloudWatchOnEC2Instance)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ InstanceId

  Type: String

  Description: \(Required\) The ID of the Amazon EC2 instance on which you want to enable CloudWatch monitoring\.
+ properties

  Type: String

  Description: \(Optional\) This parameter is not supported\. It is listed here for backwards compatibility\.
+ status

  Valid values: Enabled \| Disabled

  Description: \(Optional\) Specifies whether to enable or disable CloudWatch\.

  Default: Enabled

**Document Steps**

configureCloudWatch \- Configures CloudWatch on the Amazon EC2 instance with the given status\.

**Outputs**

The automation execution has no output\.