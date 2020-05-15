# Manually install SSM Agent on EC2 instances for Linux<a name="sysman-manual-agent-install"></a>

After you manually install SSM Agent, you can automatically update SSM Agent on your instances when new versions become available by using Systems Manager State Manager\. For more information, see [Automatically update SSM Agent \(CLI\)](sysman-state-cli.md)\.

**Important**  
These procedures apply to installing or reinstalling SSM Agent on EC2 instances for Linux\. If you need to install the agent on an on\-premises server or a virtual machine \(VM\) so it can be used with Systems Manager, see [Install SSM Agent for a hybrid environment \(Linux\)](sysman-install-managed-linux.md)\.

## Download SSM Agent from a specific AWS Region<a name="sysman-install-ssm-agent-specific"></a>

The URLs in the scripts in the topics later this section let you download SSM Agent from *any* AWS Region\. If you want to download the agent from a specific Region, copy the URL for your operating system, and then replace *region* with an appropriate value\.

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
+ Raspbian:

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_arm/amazon\-ssm\-agent\.deb

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

**Operating systems**
+ [Amazon Linux](agent-install-al.md)
+ [Amazon Linux 2](agent-install-al2.md)
+ [CentOS](agent-install-centos.md)
+ [Debian Server](agent-install-deb.md)
+ [Oracle Linux](agent-install-oracle.md)
+ [Raspbian](agent-install-raspbianjessie.md)
+ [Red Hat Enterprise Linux](agent-install-rhel.md)
+ [SUSE Linux Enterprise Server](agent-install-sles.md)
+ [Ubuntu Server](agent-install-ubuntu.md)