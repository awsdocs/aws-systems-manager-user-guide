# Installing and configuring SSM Agent on EC2 instances for macOS<a name="install-ssm-agent-macos"></a>

SSM Agent processes Systems Manager requests and configures your machine as specified in the request\. Use the following procedures to install, configure, or uninstall SSM Agent for macOS\.

**Note**  
SSM Agent is preinstalled, by default, on Amazon Machine Images AMIs for macOS\. You do not need to install SSM Agent on EC2 instance for macOS unless you have uninstalled it\.

The source code for SSM Agent is available on [GitHub](https://github.com/aws/amazon-ssm-agent) so that you can adapt the agent to meet your needs\. We encourage you to submit [pull requests](https://github.com/aws/amazon-ssm-agent/blob/mainline/CONTRIBUTING.md) for changes that you would like to have included\. However, AWS does not currently provide support for running modified copies of this software\.

**Note**  
To view details about the different versions of SSM Agent, see the [release notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md)\.

Before you manually install SSM Agent on a macOS operating system, review the following information\.
+ SSM Agent is installed by default on the following EC2 instances and Amazon Machine Images:
  + macOS 10\.14\.x \(Mojave\)
  + macOS 10\.15\.x \(Catalina\)

  SSM Agent doesn't need to be manually installed on macOS EC2 instances unless it has been uninstalled\.
+ Systems Manager does not currently support macOS in hybrid environments\.
+ An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub\.

**Topics**
+ [Manually install SSM Agent on EC2 instances for macOS](sysman-manual-agent-install-macos2.md)
+ [Configure SSM Agent to use a proxy \(macOS\)](sysman-proxy-with-ssm-agent-macos.md)
+ [Uninstall SSM Agent from macOS instances](sysman-uninstall-agent-macos.md)