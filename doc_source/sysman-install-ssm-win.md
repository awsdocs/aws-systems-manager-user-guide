# Installing and configuring SSM Agent on EC2 instances for Windows Server<a name="sysman-install-ssm-win"></a>

AWS Systems Manager Agent \(SSM Agent\) is preinstalled, by default, on the following Amazon Machine Images \(AMIs\):
+ Windows Server 2008\-2012 R2 AMIs published in November 2016 or later
+ Windows Server 2016 and 2019

**Important**  
As of January 14, 2020, Windows Server 2008 is [no longer supported](https://www.microsoft.com/en-us/cloud-platform/windows-server-2008) for feature or security updates from Microsoft\. Windows Server 2008 AMIs do include SSM Agent, but the agent is no longer updated for this operating system\.

Windows Server AMIs published *before* November 2016 use the EC2Config service to process requests and configure instances\.

Unless you have a specific reason for using the EC2Config service, or an earlier version of SSM Agent, to process Systems Manager requests, we recommend that you download and install the latest version of SSM Agent to each of your Amazon Elastic Compute Cloud \(Amazon EC2\) instances or hybrid instances that are configured for Systems Manager\.

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. Subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub to get notifications about SSM Agent updates\.

**Important**  
At present, there is a gotcha related to SSM when provisioning instances via CDK\. When assigning non\-default security groups to your EC2 instances, please ensure that the allowAllOutbound property set to true\. Otherwise, some EC2 instances may not launch with SSM preinstalled e.g. <strong>WindowsVersion.WINDOWS_SERVER_2016_ENGLISH_FULL_BASE</strong>\. This may be due to these versions being deprecated\. 

To view more information regarding deprecated versions, please see the [Windows Versions List](https://github.com/aws/aws-cdk/blob/master/packages/%40aws-cdk/aws-ec2/lib/windows-versions.ts)\.

To view details about the different versions of SSM Agent, see the [release notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md)\.

**Topics**
+ [Manually install SSM Agent on EC2 instances for Windows Server](sysman-install-win.md)
+ [Configure SSM Agent to use a proxy for Windows Server instances](sysman-install-ssm-proxy.md)
