# About SSM Agent<a name="prereqs-ssm-agent"></a>

AWS Systems Manager Agent \(SSM Agent\) is Amazon software that runs on Amazon Elastic Compute Cloud \(Amazon EC2\) instances, edge devices, and on\-premises servers and virtual machines \(VMs\)\. SSM Agent makes it possible for Systems Manager to update, manage, and configure these resources\. The agent processes requests from the Systems Manager service in the AWS Cloud, and then runs them as specified in the request\. SSM Agent then sends status and execution information back to the Systems Manager service by using the Amazon Message Delivery Service \(service prefix: `ec2messages`\)\.

SSM Agent must be installed on each instance you want to use with AWS Systems Manager\. By default, SSM Agent is preinstalled on instances created from the following Amazon Machine Images \(AMIs\):
+ Amazon Linux
+ Amazon Linux 2
+ Amazon Linux 2 ECS\-Optimized Base AMIs
+ macOS 10\.14\.x \(Mojave\), 10\.15\.x \(Catalina\), and 11\.x \(Big Sur\)
+ SUSE Linux Enterprise Server \(SLES\) 12 and 15
+ Ubuntu Server 16\.04, 18\.04, and 20\.04  
+ Windows Server 2008\-2012 R2 AMIs published in November 2016 or later
+ Windows Server 2016 and 2019

On other AMIs; AWS IoT Greengrass core devices; and on\-premises servers, edge devices, and virtual machines in your hybrid environment, you must install the agent manually, as described in the following table\.

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on a managed node, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your machines\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. Subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub to get notifications about SSM Agent updates\.


****  

| Operating system type | SSM Agent installation | 
| --- | --- | 
| Linux | SSM Agent is installed by default on Amazon Linux, Amazon Linux 2, SUSE Linux Enterprise Server \(SLES\) 12 and 15, Ubuntu Server 16\.04, 18\.04 LTS, and 20\.04 base Amazon EC2 AMIs\. You must manually install SSM Agent on other versions of Amazon EC2 for Linux, including non\-base images\. For more information, see [Installing and configuring SSM Agent on EC2 instances for Linux](sysman-install-ssm-agent.md)\. | 
| macOS |  SSM Agent is installed by default on macOS 10\.14\.6 \(Mojave\), 10\.15\.7 \(Catalina\), and 11\.x \(BigSur\) AMIs for Amazon EC2\. For more information, see [Installing and configuring SSM Agent on EC2 instances for macOS](install-ssm-agent-macos.md)\.  | 
| Windows |  Windows AMIs published before November 2016 use the EC2Config service to process requests and configure instances\. Unless you have a specific reason for using the EC2Config service or an earlier version of SSM Agent to process Systems Manager requests, we recommend that you download and install the latest version of the SSM Agent to each of your EC2 instances and managed instances in your hybrid environment\. For more information, see [Installing and configuring SSM Agent on EC2 instances for Windows Server](sysman-install-ssm-win.md)\.  | 
| Edge devices |  Systems Manager supports the following types of edge devices: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/prereqs-ssm-agent.html) Setup requirements differ based on the type of edge device\. For more information, see [Setting up AWS Systems Manager for edge devices](systems-manager-setting-up-edge-devices.md)\.  | 
| On\-premises servers and VMs |  You must manually install SSM Agent on on\-premises servers and virtual machines \(VMs\) in your hybrid environment\. The SSM Agent download and installation process for these machines is different than the process used for Amazon EC2 instances\. For more information, see the following topics: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/prereqs-ssm-agent.html)  | 