# AWS\-AttachEBSVolume<a name="automation-aws-attachebsvolume"></a>

**Description**

Attach an Amazon Elastic Block Store \(Amazon EBS\) volume to an EC2 instance\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-AttachEBSVolume)

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
+ Device

  Type: String

  Description: \(Required\) The device name \(for example, /dev/sdh or xvdh \)\.
+ InstanceId

  Type: String

  Description: \(Required\) The ID of the instance where you want to attach the volume\.
+ VolumeId

  Type: String

  Description: \(Required\) The ID of the Amazon EBS volume\. The volume and instance must be in the same Availability Zone\.