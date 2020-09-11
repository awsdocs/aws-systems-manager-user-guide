# AWS\-DetachEBSVolume<a name="automation-aws-detachebsvolume"></a>

**Description**

Detach an Amazon EBS volume from an EC2 instance\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-DetachEBSVolume)

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
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role assumed by Lambda
+ VolumeId

  Type: String

  Description: \(Required\) The ID of the EBS volume\. The volume and instance must be within the same Availability Zone