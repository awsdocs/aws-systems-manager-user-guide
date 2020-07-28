# Manually install SSM Agent on EC2 instances for Linux<a name="sysman-manual-agent-install"></a>

Before you manually install SSM Agent on an Amazon EC2 Linux operating system, review the following important details\.
+ SSM Agent is installed by default on the following EC2 instances and Amazon Machine Images:
  + Amazon Linux
  + Amazon Linux 2
  + Ubuntu Server 16\.04
  + Ubuntu Server 18\.04
  + Amazon ECS\-Optimized
+ The manual procedures enable you to install SSM Agent from *any* AWS Region\. If you want to download the agent from a specific Region, see "Download SSM Agent from a specific AWS Region" later in this topic\. That section describes how to change the URL that you specify in the manual procedure\. That section does *not* describe how to download and install the agent\. You must choose one of the operating system links and then substitute the URL to download from a specific Region\.
+ If you need to install the agent on an on\-premises server or a virtual machine \(VM\) so it can be used with Systems Manager, see [Install SSM Agent for a hybrid environment \(Linux\)](sysman-install-managed-linux.md)\.
+ An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

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

## Download SSM Agent from a specific AWS Region<a name="sysman-install-ssm-agent-specific"></a>

The manual procedures let you download SSM Agent from *any* AWS Region\. If you want to download the agent from a specific Region, copy the URL for your operating system, and then replace *region* with an appropriate value\.

*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

For example, to download SSM Agent for Amazon Linux, RHEL, CentOS, and SLES 64\-bit from the US West 1 Region, use the following URL:

```
https://s3.us-west-1.amazonaws.com/amazon-ssm-us-west-1/latest/linux_amd64/amazon-ssm-agent.rpm
```
+ Amazon Linux, RHEL, CentOS, and SLES 64\-bit:

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/linux\_amd64/amazon\-ssm\-agent\.rpm 
+ Amazon Linux, RHEL, and CentOS 32\-bit:

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/linux\_386/amazon\-ssm\-agent\.rpm
+ Ubuntu Server 64\-bit:

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_amd64/amazon\-ssm\-agent\.deb
+ Ubuntu Server 32\-bit:

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_386/amazon\-ssm\-agent\.deb