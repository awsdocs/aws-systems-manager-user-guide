# AWSEC2\-CloneInstanceAndUpgradeSQLServer<a name="automation-awsec2-CloneInstanceAndUpgradeSQLServer"></a>

**Description**

Create an AMI from an EC2 instance for Windows Server running SQL Server 2008 \(or later\), and then upgrade the AMI to SQL Server 2016\. The upgrade is a multi\-step process that can take 2 hours to complete\. The Automation creates the AMI from the instance, and then launches the new AMI in the subnet that you provide\. The Automation then performs an in\-place upgrade of SQL Server 2008 \(or later\) to SQL Server 2016\. After the upgrade, the Automation creates a new AMI before terminating the upgraded instance\. 

You can test application functionality by launching the new AMI in your VPC\. After you finish testing, and before you perform another upgrade, schedule application downtime before completely switching over to the upgraded instance\.

**Note**  
If you want to modify the computer name of the EC2 instance launched from the new AMI, see [Rename a Computer that Hosts a Stand\-Alone Instance of SQL Server](https://docs.microsoft.com/en-us/sql/database-engine/install-windows/rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server?view=sql-server-2017)\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSEC2-CloneInstanceAndUpgradeSQLServer)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows

**Parameters**

**Prerequisites**
+ The EC2 instance must use a version of Windows Server that is Windows Server 2008 R2 \(or later\) and SQL Server 2008 \(or later\)\.
+ Verify that SSM Agent is installed on your instance\. For more information, see [Installing and configuring SSM Agent on EC2 instances for Windows Server](sysman-install-ssm-win.md)\.
+ Configure the instance to use an AWS Identity and Access Management \(IAM\) instance profile role\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.
+ Verify that the instance has 20 GB of free disk space in the instance boot disk\.
+ For instances that use a Bring Your Own License \(BYOL\) SQL Server version, the following additional prerequisites apply:
  + Provide an EBS snapshot ID that includes SQL Server 2016 installation media\. To do this:

    1. Verify that the EC2 instance is running Windows Server 2008 R2 or later\.

    1. Create a 6 GB EBS volume in the same Availability Zone where the instance is running\. Attach the volume to the instance\. Mount it, for example, as drive D\. 

    1. Right\-click the ISO and mount it to an instance as, for example, drive E\.

    1. Copy the content of the ISO from drive E:\\ to drive D:\\

    1. Create an EBS snapshot of the 6 GB volume created in step 2\.

**Limitations**
+ The upgrade can only be performed on a SQL Server using Windows authentication\.
+ Verify that no security patch updates are pending on the instances\. Open **Control Panel**, then choose **Check for updates**\.
+ SQL Server deployments in HA and mirroring mode are not supported\.

**Parameters**
+ IamInstanceProfile

  Type: String

  Description: \(Required\) The IAM instance profile\.
+ InstanceId

  Type: String

  Description: \(Required\) The instance running Windows Server 2008 R2 \(or later\) and SQL Server 2008 \(or later\)\.
+ KeepPreUpgradeImageBackUp

  Type: String

  Description: \(Optional\) If set to True, the Automation doesn't delete the AMI created from the instance before the upgrade\. If set to True, then you must delete the AMI\. By default, the AMI is deleted\.
+ SubnetId

  Type: String

  Description: \(Required\) Provide a subnet for the upgrade process\. Verify that the subnet has outbound connectivity to AWS services, Amazon S3, and Microsoft \(to download patches\)\.
+ SQLServerSnapshotId

  Type: String

  Description: \(Conditional\) Snapshot ID for SQL Server 2016 installation media\. This parameter is required for instances that use a BYOL SQL Server version\. This parameter is optional for SQL Server license\-included instances \(instances launched using an AWS provided Amazon Machine Image for Windows Server with Microsoft SQL Server\)\.
+ RebootInstanceBeforeTakingImage

  Type: String

  Description: \(Optional\) If set to True, the Automation reboots the instance before creating a pre\-upgrade AMI\. By default, the Automation doesn't reboot before upgrade\.

**Outputs**

AMIId: The ID of the AMI created from the instance that was upgraded to SQL Server 2016