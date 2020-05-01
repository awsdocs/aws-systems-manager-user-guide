# AWS\-CreateImage<a name="automation-aws-createimage"></a>

**Description**

Create a new Amazon Machine Image \(AMI\) from an EC2 instance\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-CreateImage)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ InstanceId

  Type: String

  Description: \(Required\) The ID of the EC2 instance\.
+ NoReboot

  Type: Boolean

  Description: \(Optional\) Do not reboot the instance before creating the image\.