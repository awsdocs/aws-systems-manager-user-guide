# Patch Manager prerequisites<a name="patch-manager-prerequisites"></a>

Make sure that you have met the required prerequisites before using Patch Manager, a capability of AWS Systems Manager\. 

**SSM Agent version**  
Version 2\.0\.834\.0 or later of SSM Agent is running on the managed node you want to manage with Patch Manager\.

**Note**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. Failing to use the latest version of the agent can prevent your managed node from using various Systems Manager capabilities and features\. For that reason, we recommend that you automate the process of keeping SSM Agent up to date on your machines\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. Subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub to get notifications about SSM Agent updates\.

**Python version**  
For macOS and most Linux operating systems \(OSs\), Patch Manager currently supports Python versions 2\.6 \- 3\.9\. The Debian Server, Raspberry Pi OS, and Ubuntu Server OSs require a supported version of Python 3 \(3\.0 \- 3\.9\)\.

**Connectivity to the patch source**  
If your managed nodes don't have a direct connection to the Internet and you're using an Amazon Virtual Private Cloud \(Amazon VPC\) with a VPC endpoint, you must ensure that the nodes have access to the source patch repositories \(repos\)\. On Linux nodes, patch updates are typically downloaded from the remote repos configured on the node\. Therefore, the node must be able to connect to the repos so the patching can be performed\. For more information, see [How security patches are selected](patch-manager-how-it-works-selection.md)\.

Windows Server managed nodes must be able to connect to the Windows Update Catalog or Windows Server Update Services \(WSUS\)\. Confirm that your nodes have connectivity to the [Microsoft Update Catalog](https://www.catalog.update.microsoft.com/home.aspx) through an internet gateway, NAT gateway, or NAT instance\. If you are using WSUS, confirm that the node has connectivity to the WSUS server in your environment\. For more information, see [Issue: managed node doesn't have access to Windows Update Catalog or WSUS](patch-manager-troubleshooting.md#patch-manager-troubleshooting-instance-access)\.

**S3 endpoint access**  
Whether your managed nodes operate in a private or public network, without access to the required AWS managed Amazon Simple Storage Service \(Amazon S3\) buckets, patching operations fail\. For information about the S3 buckets your managed nodes must be able to access, see [SSM Agent communications with AWS managed S3 buckets](ssm-agent-minimum-s3-permissions.md) and [Step 2: Create VPC endpoints](setup-create-vpc.md)\.

**Supported operating systems**  
The Patch Manager capability doesn't support all the same operating systems versions that are supported by other Systems Manager capabilities\. For example, Patch Manager doesn't support CentOS 6\.3 or Raspberry Pi OS 8 \(Jessie\)\. \(For the full list of Systems Manager\-supported operating systems, see [Systems Manager prerequisites](systems-manager-prereqs.md)\.\) Therefore, ensure that the managed nodes you want to use with Patch Manager are running one of the operating systems listed in the following table\.


| Operating system | Details | 
| --- | --- | 
|  Linux  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-prerequisites.html) Managed nodes created from an Amazon Linux AMI that use a proxy must run a current version of the `Python` `requests` module to support Patch Manager operations\. For more information, see [Upgrading the Python requests module on Amazon Linux instances that use a proxy server](sysman-proxy-with-ssm-agent-al-python-requests.md)\.  | 
| macOS |  macOS 10\.14\.x \(Mojave\), 10\.15\.x \(Catalina\), and 11\.3\.1 & 11\.4 \(Big Sur\) SSM Agent for AWS IoT Greengrass core devices is not supported on macOS\. You can't use Patch Manager to patch macOS edge devices\.  | 
|  Windows  |  Windows Server 2008 through Windows Server 2022, including R2 versions\.  As of January 14, 2020, Windows Server 2008 is no longer supported for feature or security updates from Microsoft\. Legacy Amazon Machine Images \(AMIs\) for Windows Server 2008 and 2008 R2 still include version 2 of SSM Agent preinstalled, but Systems Manager no longer officially supports 2008 versions and no longer updates the agent for these versions of Windows Server\. In addition, [SSM Agent version 3\.0](ssm-agent-v3.md) might not be compatible with all operations on Windows Server 2008 and 2008 R2\. The final officially supported version of SSM Agent for Windows Server 2008 versions is 2\.3\.1644\.0\.  SSM Agent for AWS IoT Greengrass core devices is not supported on Windows 10\. You can't use Patch Manager to patch Windows 10 edge devices\.  | 