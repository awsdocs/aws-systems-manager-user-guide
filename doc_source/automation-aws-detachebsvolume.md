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

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role assumed by Lambda
+ VolumeId

  Type: String

  Description: \(Required\) The ID of the EBS volume\. The volume and instance must be within the same Availability Zone