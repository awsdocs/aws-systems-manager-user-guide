# AWSEC2\-CloneInstanceAndUpgradeWindows<a name="automation-awsec2-CloneInstanceAndUpgradeWindows"></a>

**Description**

Create an Amazon Machine Image \(AMI\) from a Windows Server 2008 R2, 2012 R2, or 2016 instance, and then upgrade the AMI to Windows Server 2012 R2, 2016, or 2019\. The supported upgrade paths are as follows\.
+ Windows Server 2008 R2 to Windows Server 2012 R2\.
+ Windows Server 2012 R2 to Windows Server 2016\.
+ Windows Server 2012 R2 to Windows Server 2019\.
+ Windows Server 2016 to Windows Server 2019\.

To upgrade your instance from Windows Server 2008 R2 to Windows Server 2016 or 2019, the Automation document performs two steps\. The Windows Server 2008 R2 instance is upgraded to Windows Server 2012 R2\. Then the Windows Server 2012 R2 instance is upgraded to the target version \(Windows Server 2016 or 2019\)\.

The upgrade operation is a multi\-step process that can take 2 hours to complete\. The Automation creates an AMI from the instance and then launches a temporary instance from the newly created AMI in the `SubnetId` that you specify\. The security groups associated with your original instance are applied to the temporary instance\. The Automation workflow then performs an in\-place upgrade to the `TargetWindowsVersion` on the temporary instance\. To upgrade your Windows Server 2008 R2 instance to Windows Server 2016 or 2019, an in\-place upgrade is performed twice because directly upgrading Windows Server 2008 R2 to Windows Server 2016 or 2019 is not supported\. The workflow also updates or installs the AWS drivers required by the temporary instance\. After the upgrade, the workflow creates a new AMI from the temporary instance and then terminates the temporary instance\.

You can test application functionality by launching a test instance from the upgraded AMI in your VPC\. After you finish testing, and before you perform another upgrade, schedule application downtime before completely switching over to the upgraded AMI\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSEC2-CloneInstanceAndUpgradeWindows)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows Server 2008 R2, 2012 R2 and 2016 Standard and Datacenter editions

**Prerequisites**
+ Verify that SSM Agent is installed on your instance\. For more information, see [Installing and configuring SSM Agent on EC2 instances for Windows Server](sysman-install-ssm-win.md)\.
+ For instances that are joined to a Microsoft Active Directory domain, we recommend specifying a `SubnetId` that does not have connectivity to your domain controllers to help avoid hostname conflicts\.
+ The `SubnetId` specified must be a public subnet with the auto\-assign public IPv4 address set to true\. For more information, see [Modifying the Public IPv4 Addressing Attribute for Your Subnet](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-ip-addressing.html#subnet-public-ip) in the *Amazon VPC User Guide*\.
+ This Automation works only with Windows Server 2008 R2, 2012 R2, and 2016 instances\.
+ This Automation works only on instances with an unencrypted EBS root volume\. If the specified instance has an encrypted root volume, the Automation workflow fails\.
+ Configure the Windows Server instance with an AWS Identity and Access Management \(IAM\) instance profile that provides the requisite permissions for Systems Manager\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.
+ Verify that the instance has 20 GB of free disk space in the boot disk\.
+ If the instance does not use an AWS\-provided Windows license, then specify an Amazon EBS snapshot ID that includes Windows Server 2012 R2 installation media\. To do this:
  + Verify that the EC2 instance is running Windows Server 2012 or later\.
  + Create a 6 GB EBS volume in the same Availability Zone where the instance is running\. Attach the volume to the instance\. Mount it, for example, as drive D\. 
  + Right\-click the ISO and mount it to an instance as, for example, drive E\.
  + Copy the content of the ISO from drive E:\\ to drive D:\\
  + Create an EBS snapshot of the 6 GB volume created in step 2 above\.

**Limitations**

This Automation doesn't support upgrading Windows domain controllers, clusters, or Windows desktop operating systems\. This Automation also doesn't support EC2 instances for Windows Server with the following roles installed\.
+ Remote Desktop Session Host \(RDSH\)
+ Remote Desktop Connection Broker \(RDCB\)
+ Remote Desktop Virtualization Host \(RDVH\)
+ Remote Desktop Web Access \(RDWA\)

**Parameters**
+ BYOLWindowsMediaSnapshotId

  Type: String

  Description: \(Optional\) The ID of the Amazon EBS snapshot to copy that includes Windows Server 2012R2 installation media\. Required only if you are upgrading a BYOL instance\.
+ IamInstanceProfile

  Type: String

  Description: \(Required\) The name of the IAM instance profile that enables Systems Manager to manage the instance\.
+ InstanceId

  Type: String

  Description: \(Required\) The instance running Windows Server 2008 R2 or 2012 R2\.
+ KeepPreUpgradeImageBackUp

  Type: String

  Description: \(Optional\) If set True, the Automation doesn't delete the AMI created from the instance before the upgrade\. If set to True, then you must delete the AMI\. By default, the AMI is deleted\.
+ SubnetId

  Type: String

  Description: \(Required\) Provide a subnet for the upgrade process\. Verify that the subnet has outbound connectivity to AWS services, Amazon S3, and Microsoft \(to download patches\)\.
+ TargetWindowsVersion

  Type: String

  Description: \(Required\) Select the target Windows version\.

  Default: 2012R2
+ RebootInstanceBeforeTakingImage

  Type: String

  Description: \(Optional\) If set True, the Automation reboots the instance before creating a pre\-upgrade AMI\. By default, the Automation doesn't reboot before upgrade\.