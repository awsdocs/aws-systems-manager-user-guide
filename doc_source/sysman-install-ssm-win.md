# Installing and Configuring SSM Agent on Windows Instances<a name="sysman-install-ssm-win"></a>

SSM Agent is installed by default on Windows Server 2016 instances and instances created from Windows Server 2003\-2012 R2 AMIs published in November 2016 or later\.

Windows AMIs published *before* November 2016 use the EC2Config service to process requests and configure instances\.

Unless you have a specific reason for using the EC2Config service, or an earlier version of SSM Agent, to process Systems Manager requests, we recommend that you download and install the latest version of SSM Agent to each of your Amazon EC2 instances or hybrid instances that are configured for Systems Manager\.

If you need to update SSM Agent, we recommend that you use State Manager to automatically update SSM Agent on your instances when new versions become available\. For more information, see [Walkthrough: Automatically Update SSM Agent \(CLI\)](sysman-state-cli.md)\.

To view details about the different versions of SSM Agent, see the [release notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md)\.

**Topics**
+ [Install and Configure SSM Agent on Windows Instances](sysman-install-win.md)
+ [View SSM Agent Logs on Windows Instances](sysman-agent-logs-win.md)
+ [Configure SSM Agent to Use a Proxy for Windows Instances](sysman-install-ssm-proxy.md)