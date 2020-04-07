# AWSEC2\-CloneInstanceAndUpgradeWindows<a name="automation-awsec2-CloneInstanceAndUpgradeWindows"></a>

**Description**

Create an Amazon Machine Image \(AMI\) from a Windows Server 2008 R2 or 2012 R2 instance, and then upgrade the AMI to Windows Server 2012 R2 or Windows Server 2019\. The supported upgrade paths are as follows\.
+ Windows Server 2008 R2 to Windows Server 2012 R2\.
+ Windows Server 2012 R2 to Windows Server 2019\.

To upgrade your instance from Windows Server 2008 R2 to Windows Server 2019, the Automation document performs two steps\. The Windows Server 2008 R2 instance is upgraded to Windows Server 2012 R2\. Then the Windows Server 2012 R2 instance is upgraded to Windows Server 2019\.

**Important**  
This Automation document does not support Windows Server 2008 R2 or 2012 R2 Enterprise editions\. This document only supports Windows Server 2008 R2 and 2012 R2 Standard and Datacenter editions\.

The upgrade operation is a multi\-step process that can take 2 hours to complete\. The Automation creates an AMI from the instance and then launches the newly created AMI in the VPC or subnet that you provide\. The Automation workflow performs an in\-place upgrade to the `TargetWindowsVersion`\. To upgrade your Windows Server 2008 R2 instance to Windows Server 2019, an in\-place upgrade is performed twice because directly upgrading Windows Server 2008 R2 to Windows Server 2019 is not supported\. The workflow also updates or installs the AWS drivers required by the upgraded instance\. After the upgrade, the workflow creates a new AMI and then terminates the upgraded instance\.

You can test application functionality by launching the new AMI in your VPC\. After you finish testing, and before you perform another upgrade, schedule application downtime before completely switching over to the upgraded instance\.

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows Server 2008 R2 and 2012 R2 Standard and Datacenter editions

**Prerequisites**
+ Verify that SSM Agent is installed on your instance\. For more information, see [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md)\.
+ You must specify a different VPC or subnet ID for the cloned instance\. If you don't, Systems Manager creates a new instance with the same name in the current VPC or subnet\. This results in a name conflict\.
+ The subnet ID specified must be a public subnet with the auto\-assign public IPv4 address set to true\. For more information, see [Modifying the Public IPv4 Addressing Attribute for Your Subnet](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-ip-addressing.html#subnet-public-ip) in the *Amazon VPC User Guide*\.
+ This Automation works only with Windows Server 2008 R2 and Windows Server 2012 R2 instances\.
+ This Automation works only on instances with an unencrypted EBS root volume\. If the specified instance has an encrypted root volume, the Automation workflow fails\.
+ Configure the Windows Server instance with an AWS Identity and Access Management \(IAM\) instance profile role\. For more information, see [Create an IAM Instance Profile for Systems Manager](setup-instance-profile.md)\.
+ Verify that the instance has 20 GB of free disk space in the boot disk\.
+ If the instance does not use an AWS\-provided Windows license, then specify an Amazon EBS snapshot ID that includes Windows Server 2012 R2 installation media\. To do this:
  + Verify that the Amazon EC2 instance is running Windows Server 2012 or later\.
  + Create a 6 GB EBS volume in the same Availability Zone where the instance is running\. Attach the volume to the instance\. Mount it, for example, as drive D\. 
  + Right\-click the ISO and mount it to an instance as, for example, drive E\.
  + Copy the content of the ISO from drive E:\\ to drive D:\\
  + Create an EBS snapshot of the 6 GB volume created in step 2 above\.

**Limitations**

This Automation doesn't support upgrading Windows domain controllers, clusters, or Windows desktop operating systems\. This Automation also doesn't support Windows instances with the following roles installed\.
+ Remote Desktop Session Host \(RDSH\)
+ Remote Desktop Connection Broker \(RDCB\)
+ Remote Desktop Virtualization Host \(RDVH\)
+ Remote Desktop Web Access \(RDWA\)

**Parameters**
+ InstanceId

  Type: String

  Description: \(Required\) The instance running Windows Server 2008 R2 or 2012 R2\.
+ IamInstanceProfile

  Type: String

  Description: \(Required\) The name of the IAM instance profile that enables Systems Manager to manage the instance\.
+ SubnetId

  Type: String

  Description: \(Required\) Provide a subnet for the upgrade process\. Verify that the subnet has outbound connectivity to AWS services, Amazon S3, and Microsoft \(to download patches\)\.
+ TargetWindowsVersion

  Type: String

  Description: \(Required\) Select the target Windows version\.

  Default: 2012R2
+ BYOLWindowsMediaSnapshotId

  Type: String

  Description: \(Optional\) The ID of the Amazon EBS snapshot to copy that includes Windows Server 2012R2 installation media\. Required only if you are upgrading a BYOL instance\.
+ KeepPreUpgradeImageBackUp

  Type: String

  Description: \(Optional\) If set True, the Automation doesn't delete the AMI created from the instance before the upgrade\. If set to True, then you must delete the AMI\. By default, the AMI is deleted\.
+ RebootInstanceBeforeTakingImage

  Type: String

  Description: \(Optional\) If set True, the Automation reboots the instance before creating a pre\-upgrade AMI\. By default, the Automation doesn't reboot before upgrade\.