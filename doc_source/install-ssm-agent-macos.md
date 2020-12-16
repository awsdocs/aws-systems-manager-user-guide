# Installing and configuring SSM Agent on EC2 instances for macOS<a name="install-ssm-agent-macos"></a>

SSM Agent processes Systems Manager requests and configures your machine as specified in the request\. Use the following procedures to install, configure, or uninstall SSM Agent for macOS\.

**Note**  
SSM Agent is preinstalled, by default, on Amazon Machine Images AMIs for macOS\. You do not need to install SSM Agent on EC2 instance for macOS unless you have uninstalled it\.

The source code for SSM Agent is available on [GitHub](https://github.com/aws/amazon-ssm-agent) so that you can adapt the agent to meet your needs\. We encourage you to submit [pull requests](https://github.com/aws/amazon-ssm-agent/blob/master/CONTRIBUTING.md) for changes that you would like to have included\. However, AWS does not currently provide support for running modified copies of this software\.

**Note**  
To view details about the different versions of SSM Agent, see the [release notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md)\.

Before you manually install SSM Agent on a macOS operating system, review the following information\.
+ SSM Agent is installed by default on the following EC2 instances and Amazon Machine Images:
  + macOS 10\.14\.x \(Mojave\)
  + macOS 10\.15\.x \(Catalina\)

  SSM Agent doesn't need to be manually installed on macOS EC2 instances unless it has been uninstalled\.
+ The manual procedures enable you to install SSM Agent from *any* AWS Region\. If you want to download the agent from a specific Region, see "Download SSM Agent from a specific AWS Region" later in this topic\. That section describes how to change the URL that you specify in the manual procedure\. That section does *not* describe how to download and install the agent\. You must choose one of the operating system links and then substitute the URL to download from a specific Region\.
+ Systems Manager does not currently support macOS in hybrid environments\.
+ An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub\.

**Topics**
+ [Download SSM Agent from a specific AWS Region](#sysman-install-ssm-agent-specific)
+ [Manually install SSM Agent on EC2 instances for macOS](sysman-manual-agent-install-macos2.md)
+ [Configure SSM Agent to use a proxy \(macOS\)](sysman-proxy-with-ssm-agent-macos.md)
+ [Uninstall SSM Agent from macOS instances](sysman-uninstall-agent-macos.md)

## Download SSM Agent from a specific AWS Region<a name="sysman-install-ssm-agent-specific"></a>

The manual procedures let you download SSM Agent from *any* AWS Region\. If you want to download the agent from a specific Region, copy the URL for your operating system, and then replace *region* with an appropriate value\.

*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

For example, to download SSM Agent for macOS from the US East \(Ohio\) Region \(us\-east\-2\), use the following URL:

```
https://s3.us-east-2.amazonaws.com/amazon-ssm-us-east-2/latest/darwin_amd64/amazon-ssm-agent.pkg
```