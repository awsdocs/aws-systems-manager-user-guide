# AWSEC2\-CloneInstanceAndUpgradeSQLServer<a name="automation-awsec2-CloneInstanceAndUpgradeSQLServer"></a>

**Description**

Create an AMI from an Amazon EC2 Windows instance running SQL Server 2008 R2 SP3, and then upgrade the AMI to SQL Server 2016\. The upgrade is a multi\-step process that can take 2 hours to complete\. The Automation creates the AMI from the instance, and then launches the new AMI in the subnet that you provide\. The Automation then performs an in\-place upgrade of SQL Server 2008 R2 to SQL Server 2016\. After the upgrade, the Automation creates a new AMI before terminating the upgraded instance\. 

You can test application functionality by launching the new AMI in your VPC\. After you finish testing, and before you perform another upgrade, schedule application downtime before completely switching over to the upgraded instance\.

**Document Type**

Automation

**Owner**

Amazon

**Platform\(s\)**

Windows

**Prerequisites**
+ The Amazon EC2 instance must use a version of Windows Server that is later than Windows Server 2008 R2 and SQL Server 2008 R2 SP3\.
+ Verify that SSM Agent is installed on your instance\. For more information, see [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md)\.
+ Configure the instance to use an AWS Identity and Access Management \(IAM\) instance profile role\. For more information, see [](sysman-configuring-access-role.md)\.
+ Verify that the instance has 20 GB of free disk space in the instance boot disk\.
+ Provide an EBS Snapshot ID that includes SQL Server 2016 installation media\. To do this:
  + Verify that the Amazon EC2 instance is running Windows Server 2008 R2 or later\.
  + Create a 6 GB EBS volume in the same Availability Zone where the instance is running\. Attach the volume to the instance\. Mount it, for example, as drive D\. 
  + Right\-click the ISO and mount it to an instance as, for example, drive E\.
  + Copy the content of the ISO from drive E:\\ to drive D:\\
  + Create an EBS snapshot of the 6 GB volume created in step 2 above\.

**Limitations**
+ This Automation only supports upgrading instances that use a Bring Your Own License SQL Server version\.
+ The upgrade can only be performed on a SQL Server using Windows authentication\.
+ Verify that no security patch updates are pending on the instances\. Open **Control Panel**, then choose **Check for updates**\.
+ SQL Server deployments in HA and mirroring mode are not supported\.

**Parameters**
+ InstanceId

  Type: String

  Description: \(Required\) The Instance running Windows Server 2008 R2 \(or later\) or SQL Server 2008 R2 \(or later\)\.
+ IamInstanceProfile

  Type: String

  Description: \(Required\) The IAM instance profile\.
+ SnapshotId

  Type: String

  Description: \(Required\) SnapshotId for SQL Server 2016 installation media\.
+ SubnetId

  Type: String

  Description: \(Required\) Provide a subnet for the upgrade process\. Verify that the subnet has outbound connectivity to AWS services, Amazon S3, and Microsoft \(to download patches\)\.
+ KeepPreUpgradeImageBackUp

  Type: String

  Description: \(Optional\) If set True, the Automation doesn't delete the AMI created from the instance before the upgrade\. If set to True, then you must delete the AMI\. By default, the AMI is deleted\.
+ RebootInstanceBeforeTakingImage

  Type: String

  Description: \(Optional\) If set True, the Automation reboots the instance before creating a pre\-upgrade AMI\. By default, the Automation doesn't reboot before upgrade\.

**Outputs**

AMIId: The ID of the AMI created from the instance that was upgraded to SQL Server 2016