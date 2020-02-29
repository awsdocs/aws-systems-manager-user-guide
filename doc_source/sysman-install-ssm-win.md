# Installing and Configuring SSM Agent on Windows Instances<a name="sysman-install-ssm-win"></a>

SSM Agent is installed by default on instances created from Windows Server 2016 and Windows Server 2019 Amazon Machine Images \(AMIs\), and on instances created from Windows Server 2008\-2012 R2 AMIs published in November 2016 or later\.

Windows AMIs published *before* November 2016 use the EC2Config service to process requests and configure instances\.

Unless you have a specific reason for using the EC2Config service, or an earlier version of SSM Agent, to process Systems Manager requests, we recommend that you download and install the latest version of SSM Agent to each of your Amazon EC2 instances or hybrid instances that are configured for Systems Manager\.

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate Updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

To view details about the different versions of SSM Agent, see the [release notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md)\.

**Topics**
+ [Install and Configure SSM Agent on Amazon EC2 Windows Instances](sysman-install-win.md)
+ [View SSM Agent Logs on Windows Instances](sysman-agent-logs-win.md)
+ [Configure SSM Agent to Use a Proxy for Windows Instances](sysman-install-ssm-proxy.md)