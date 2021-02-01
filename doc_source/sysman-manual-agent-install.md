# Manually install SSM Agent on EC2 instances for Linux<a name="sysman-manual-agent-install"></a>

Before you manually install SSM Agent on an Amazon EC2 Linux operating system, review the following information\.
+ SSM Agent is installed, by default, on the following EC2 instances and Amazon Machine Images \(AMIs\):
  + Amazon Linux
  + Amazon Linux 2
  + Amazon Linux 2 ECS\-Optimized AMIs
  + Ubuntu Server 16\.04, 18\.04, and 20\.04  
**Note**  
SSM Agent is not installed on all AMIs based on Amazon Linux or Amazon Linux 2\. For example, SSM Agent is not preinstalled on EKS\-Optimized AMIs based on Amazon Linux 2\.
+ The manual procedures enable you to install SSM Agent from *any* AWS Region\.
+ If you need to install the agent on an on\-premises server or a virtual machine \(VM\) so it can be used with Systems Manager, see [Install SSM Agent for a hybrid environment \(Linux\)](sysman-install-managed-linux.md)\.
+ An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub\.

**Choose your operating system**  
Choose a link to view the procedure for manually installing SSM Agent on the specified operating system\. 
+ [Amazon Linux](agent-install-al.md)
+ [Amazon Linux 2](agent-install-al2.md)
+ [CentOS](agent-install-centos.md)
+ [Debian Server](agent-install-deb.md)
+ [Oracle Linux](agent-install-oracle.md)
+ [Red Hat Enterprise Linux](agent-install-rhel.md)
+ [SUSE Linux Enterprise Server](agent-install-sles.md)
+ [Ubuntu Server](agent-install-ubuntu.md)