# Installing and Configuring SSM Agent on Windows Instances<a name="sysman-install-ssm-win"></a>

SSM Agent is installed by default on Windows Server 2016 instances and instances created from Windows Server 2003\-2012 R2 AMIs published in November 2016 or later\.

Windows AMIs published *before* November 2016 use the EC2Config service to process requests and configure instances\.

Unless you have a specific reason for using the EC2Config service, or an earlier version of SSM Agent, to process Systems Manager requests, we recommend that you download and install the latest version of SSM Agent to each of your Amazon EC2 instances or hybrid instances that are configured for Systems Manager\.

**Note**  
SSM Agent is updated whenever changes are made to Systems Manager and when new capabilities are added\. To ensure that your instances are always running the newest version of SSM Agent, we recommend that you update the agent automatically whenever a new version is available using either of the following methods\.  
Use a State Manager association\. For information, see the State Manager topic [Automatically Update SSM Agent \(CLI\)](sysman-state-cli.md)\.
Use a Maintenance Window\. For information, see the Maintenance Window topics [Automatically Update SSM Agent \(AWS CLI\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/mw-walkthrough-cli.html) and [Automatically Update SSM Agent \(Console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/mw-walkthrough-console.html)\. 
If you prefer to update SSM Agent on your instances manually, you can subscribe to notifications that AWS publishes when a new version of the agent is released\. For information, see [Subscribing to SSM Agent Notifications](ssm-agent-subscribe-notifications.md)\. For information about using Run Command to manually update one or more instances with the latest version, see [Update SSM Agent by using Run Command](rc-console.md#rc-console-agentexample)\.

To view details about the different versions of SSM Agent, see the [release notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md)\.

**Topics**
+ [Install and Configure SSM Agent on Amazon EC2 Windows Instances](sysman-install-win.md)
+ [View SSM Agent Logs on Windows Instances](sysman-agent-logs-win.md)
+ [Configure SSM Agent to Use a Proxy for Windows Instances](sysman-install-ssm-proxy.md)