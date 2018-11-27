# AWSEC2\-CloneInstanceAndUpgradeWindows<a name="automation-awsec2-CloneInstanceAndUpgradeWindows"></a>

**Description**

Create an Amazon Machine Image \(AMI\) from a Windows Server 2008 R2 instance, and then upgrade the AMI to Windows Server 2012\. The upgrade operation is a multi\-step process that can take 2 hours to complete\. The automation creates an AMI from the instance and then launches the newly created AMI in the VPC/Subnet you provide\. The Automation workflow performs an in\-place upgrade from Windows Server 2008 R2 to Windows server 2012 R2\. The workflow also updates or installs the AWS drivers required by the upgraded instance\. After the upgrade, the workflow creates a new AMI and then terminates the upgraded instance\. 

You can test application functionality by launching the new AMI in your VPC\. After you finish testing, and before you perform another upgrade, schedule application downtime before completely switching over to the upgraded instance\.

**Document Type**

Automation

**Owner**

Amazon

**Platform\(s\)**

Windows

**Prerequisites**
+ Verify that SSM Agent is installed on your instance\. For more information, see [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md)\.
+ This Automation works only with Windows Server 2008 R2 instances\.
+ Configure the Windows Server 2008 R2 instance with an AWS Identity and Access Management \(IAM\) instance profile role\. For more information, see [](sysman-configuring-access-role.md)\.
+ Verify that the instance has 20 GB of free disk space in the boot disk\.
+ If the instance does not use an AWS\-provided Windows license, then specify an EBS Snapshot ID that includes Windows Server 2012 R2 installation media\. To do this:
  + Verify that the Amazon EC2 instance is running Windows Server 2012 or later\.
  + Create a 6 GB EBS volume in the same Availability Zone where the instance is running\. Attach the volume to the instance\. Mount it, for example, as drive D\. 
  + Right\-click the ISO and mount it to an instance as, for example, drive E\.
  + Copy the content of the ISO from drive E:\\ to drive D:\\
  + Create an EBS snapshot of the 6 GB volume created in step 2 above\.

**Limitations**

This Automation doesn't support upgrading Windows domain controllers, clusters, or Windows work stations\.

**Parameters**
+ InstanceId

  Type: String

  Description: \(Required\) The instance running Windows Server 2008 R2\.
+ IamInstanceProfile

  Type: String

  Description: \(Required\) The IAM instance profile\.
+ SubnetId

  Type: String

  Description: \(Required\) Provide a subnet for the upgrade process\. Verify that the subnet has outbound connectivity to AWS services, Amazon S3, and Microsoft \(to download patches\)\.
+ BYOLWindowsMediaSnapshotId

  Type: String

  Description: \(Optional\) The ID of the Amazon EBS snapshot to copy that includes Windows Server 2012 R2 installation media\. Required only if you are upgrading a BYOL instance\.
+ KeepPreUpgradeImageBackUp

  Type: String

  Description: \(Optional\) If set True, the Automation doesn't delete the AMI created from the instance before the upgrade\. If set to True, then you must delete the AMI\. By default, the AMI is deleted\.
+ RebootInstanceBeforeTakingImage

  Type: String

  Description: \(Optional\) If set True, the Automation reboots the instance before creating a pre\-upgrade AMI\. By default, the Automation doesn't reboot before upgrade\.