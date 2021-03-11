# Patch Manager prerequisites<a name="patch-manager-prerequisites"></a>

Make sure that you have met the required prerequisites before using Patch Manager\. 

**SSM Agent version**  
Version 2\.0\.834\.0 or later of SSM Agent is running on the instances you want to manage with Patch Manager\.

**Note**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub\.

**Supported operating systems**  
The Patch Manager capability does not support all the same operating systems versions that are supported by other AWS Systems Manager capabilities\. For example, Patch Manager does not support CentOS 6\.3 or Raspbian Stretch\. \(For the full list of Systems Manager\-supported operating systems, see [Systems Manager prerequisites](systems-manager-prereqs.md)\.\) Therefore, ensure that the instances you want to use with Patch Manager are running one of the operating systems listed in the following table\.


| Operating system | Details | 
| --- | --- | 
|  Linux  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-prerequisites.html) Instances created from an Amazon Linux AMI that are using a proxy must be running a current version of the Python `requests` module in order to support Patch Manager operations\. For more information, see [Upgrade the Python requests module on Amazon Linux instances that use a proxy server](sysman-proxy-with-ssm-agent-al-python-requests.md)\.  | 
| macOS |  macOS 10\.14\.x \(Mojave\) and 10\.15\.x \(Catalina\)  | 
|  Windows  |  Windows Server 2008 through Windows Server 2019, including R2 versions\.  As of January 14, 2020, Windows Server 2008 is [no longer supported](https://www.microsoft.com/en-us/cloud-platform/windows-server-2008) for feature or security updates from Microsoft\. Legacy Amazon Machine Images \(AMIs\) for Windows Server 2008 and 2008 R2 still include version 2 of SSM Agent preinstalled, but Systems Manager no longer officially supports 2008 versions and no longer updates the agent for these versions\. In addition, [SSM Agent version 3](ssm-agent-v3.md) is not compatible with Windows Server 2008 and 2008 R2\. The final supported version of SSM Agent for Windows Server 2008 versions is 2\.3\.1644\.0\.   | 