# Manually installing SSM Agent on Ubuntu Server instances<a name="agent-install-ubuntu"></a>

**Important**  
Before you install SSM Agent on a 64\-bit version of Ubuntu Server, ensure sure that you are using the correct installation tools\. Beginning with Amazon Machine Images \(AMIs\) that are identified with 20180627, SSM Agent is pre\-installed on version 16\.04 using Snap packages\. On instances created from earlier AMIs, SSM Agent must be installed using deb installer packages\. For more information, see [Determining the correct SSM Agent version to install on 64\-bit Ubuntu Server 16\.04 instances](agent-install-ubuntu-about-v16.md)

In most cases, the Amazon Machine Images \(AMIs\) for Ubuntu Server that are provided by AWS come with AWS Systems Manager Agent \(SSM Agent\) preinstalled by default\. For more information, see [Amazon Machine Images \(AMIs\) with SSM Agent preinstalled](ami-preinstalled-agent.md)\.

In the event that SSM Agent isn’t preinstalled on a new Ubuntu Server instance, or if you need to manually reinstall the agent, use the information in this section to help you\.

**Before you begin**  
Before you install SSM Agent on an Ubuntu Server instance, note the following:
+ For important information that applies to installation of SSM Agent on all Linux\-based operating systems, see [Manually installing SSM Agent on EC2 instances for Linux](sysman-manual-agent-install.md)\.

**Topics**
+ [Install SSM Agent on Ubuntu Server 22\.04 LTS, 20\.10 STR & 20\.04, 18\.04, and 16\.04 LTS 64\-bit \(Snap\)](agent-install-ubuntu-64-snap.md)
+ [Install SSM Agent on Ubuntu Server 16\.04 and 14\.04 64\-bit \(deb\)](agent-install-ubuntu-64-deb.md)
+ [Install SSM Agent on Ubuntu Server 16\.04 and 14\.04 32\-bit](agent-install-ubuntu-32.md)
+ [Determining the correct SSM Agent version to install on 64\-bit Ubuntu Server 16\.04 instances](agent-install-ubuntu-about-v16.md)