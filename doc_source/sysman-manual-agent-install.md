# Manually installing SSM Agent on EC2 instances for Linux<a name="sysman-manual-agent-install"></a>

Before you manually install AWS Systems Manager Agent \(SSM Agent\) on an Amazon Elastic Compute Cloud \(Amazon EC2\) Linux operating system, review the following information\.

**SSM Agent installation file URLs**  
You can access the installation files for SSM Agent that are stored in any commercial AWS Region\. We also provide installation files in a globally available Amazon Simple Storage Service \(Amazon S3\) bucket that you can use as an alternative or backup source of files\.

If you're manually installing the agent on a instance or two, you can use the commands in the **Quick installation** procedures we provide to save time\. 

If you're creating a script or template to use for installing the agent on multiple instances, we recommend that you use the installation files in or near an AWS Region where you're geographically located\. For bulk installations, this can increase the speed of your downloads and reduce latency\. In these cases, we recommend using the **Create custom installation commands** procedures in the installation topics\.

**Amazon Machine Images with the agent preinstalled**  
SSM Agent is preinstalled on some Amazon Machine Images \(AMIs\) provided by AWS\. For information, see [Amazon Machine Images \(AMIs\) with SSM Agent preinstalled](ami-preinstalled-agent.md)\.

**Installation on other machine types**  
If you need to install the agent on an on\-premises server or a virtual machine \(VM\) so that it can be used with Systems Manager, see [Install SSM Agent for a hybrid environment \(Linux\)](sysman-install-managed-linux.md)\. For information about installing the agent on edge devices, see [Setting up AWS Systems Manager for edge devices](systems-manager-setting-up-edge-devices.md)\.

**Keeping the agent up to date**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. Failing to use the latest version of the agent can prevent your managed node from using various Systems Manager capabilities and features\. For that reason, we recommend that you automate the process of keeping SSM Agent up to date on your machines\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. Subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub to get notifications about SSM Agent updates\.

**Choose your operating system**  
To view the procedure for manually installing SSM Agent on the specified operating system, choose a link from the following list: 

**Note**  
For a list of supported versions of each of the following operating systems, see [Supported operating systems](prereqs-operating-systems.md)\.
+ [Amazon Linux](agent-install-al.md)
+ [Amazon Linux 2](agent-install-al2.md)
+ [CentOS](agent-install-centos.md)
+ [CentOS Stream](agent-install-centos-stream.md)
+ [Debian Server](agent-install-deb.md)
+ [Oracle Linux](agent-install-oracle.md)
+ [Red Hat Enterprise Linux](agent-install-rhel.md)
+ [Rocky Linux](agent-install-rocky.md)
+ [SUSE Linux Enterprise Server](agent-install-sles.md)
+ [Ubuntu Server](agent-install-ubuntu.md)