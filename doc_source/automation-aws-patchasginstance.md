# AWS\-PatchAsgInstance<a name="automation-aws-patchasginstance"></a>

**Description**

Patch EC2 instances in an Auto Scaling group\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-PatchAsgInstance)

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

  Description: \(Required\) ID of the instance to patch\. Don't specify an instance ID that is configured to run during a Maintenance Window\.
+ LambdaRoleArn

  Type: String

  Description: \(Optional\) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf\. If not specified a transient role will be created to run the Lambda function\.
+ WaitForInstance

  Type: String

  Default: PT2M

  Description: \(Optional\) Duration the Automation should sleep to allow the instance to come back into service\.
+ WaitForReboot

  Type: String

  Default: PT5M

  Description: \(Optional\) Duration the Automation should sleep to allow a patched instance to reboot\.