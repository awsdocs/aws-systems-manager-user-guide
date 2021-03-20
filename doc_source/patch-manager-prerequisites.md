# Patch Manager prerequisites<a name="patch-manager-prerequisites"></a>

Make sure that you have met the required prerequisites before using Patch Manager\. 

**SSM Agent version**  
Version 2\.0\.834\.0 or later of SSM Agent is running on the instances you want to manage with Patch Manager\.

**Note**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub\.

**Connectivity to the patch source**  
If your instances don't have a direct connection to the Internet and you are using an Amazon Virtual Private Cloud \(Amazon VPC\) with a VPC endpoint, you must ensure that the instances have access to the source patch repositories \(repos\)\. On Linux instances, patch updates are typically downloaded from the remote repos configured on the instance\. Therefore, the instance must be able to connect to the repos so the patching can be performed\. For more information, see [How security patches are selected](patch-manager-how-it-works-selection.md)\.

Windows Server instances must be able to connect to the Windows Update Catalog or Windows Server Update Services \(WSUS\)\. Confirm that your instances have connectivity to the [Microsoft Update Catalog](https://www.catalog.update.microsoft.com/home.aspx) through an internet gateway, NAT gateway, or NAT instance\. If you are using WSUS, confirm that the instance has connectivity to the WSUS server in your environment\. For more information, see [Troubleshooting instance does not have access to Windows Update Catalog or WSUS](patch-manager-troubleshooting.md#patch-manager-troubleshooting-instance-access)\.

**S3 endpoint access**  
Whether your instances operate in a private or public network, without access to the required AWS managed S3 buckets, patching operations fail\. For information about the S3 buckets your managed instances must be able to access, see [About minimum S3 Bucket permissions for SSM Agent](ssm-agent-minimum-s3-permissions.md) and [Step 6: \(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md)\.

**Supported operating systems**  
The Patch Manager capability does not support all the same operating systems versions that are supported by other AWS Systems Manager capabilities\. For example, Patch Manager does not support CentOS 6\.3 or Raspbian Stretch\. \(For the full list of Systems Manager\-supported operating systems, see [Systems Manager prerequisites](systems-manager-prereqs.md)\.\) Therefore, ensure that the instances you want to use with Patch Manager are running one of the operating systems listed in the following table\.


| Operating system | Details | 
| --- | --- | 
|  Linux  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-prerequisites.html) Instances created from an Amazon Linux AMI that are using a proxy must be running a current version of the Python `requests` module in order to support Patch Manager operations\. For more information, see [Upgrade the Python requests module on Amazon Linux instances that use a proxy server](sysman-proxy-with-ssm-agent-al-python-requests.md)\.  | 
| macOS |  macOS 10\.14\.x \(Mojave\) and 10\.15\.x \(Catalina\)  | 
|  Windows  |  Windows Server 2008 through Windows Server 2019, including R2 versions\.  As of January 14, 2020, Windows Server 2008 is [no longer supported](https://www.microsoft.com/en-us/cloud-platform/windows-server-2008) for feature or security updates from Microsoft\. Legacy Amazon Machine Images \(AMIs\) for Windows Server 2008 and 2008 R2 still include version 2 of SSM Agent preinstalled, but Systems Manager no longer officially supports 2008 versions and no longer updates the agent for these versions of Windows Server\. In addition, [SSM Agent version 3](ssm-agent-v3.md) may not be compatible with all operations on Windows Server 2008 and 2008 R2\. The final officially supported version of SSM Agent for Windows Server 2008 versions is 2\.3\.1644\.0\.   | 