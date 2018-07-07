# Installing and Configuring SSM Agent on Linux Instances<a name="sysman-install-ssm-agent"></a>

The SSM Agent processes Systems Manager requests and configures your machine as specified in the request\. Use the following procedures to install, configure, or uninstall SSM Agent\.

**Important**  
SSM Agent is installed, by default, on Amazon Linux *base* AMIs dated 2017\.09 and later\. SSM Agent is also installed, by default, on Amazon Linux 2, Ubuntu Server 16\.04, and Ubuntu Server 18\.04 LTS AMIs\.
You must manually install SSM Agent on other versions of Linux, including non\-base images like *Amazon ECS\-Optimized AMIs*\.

The source code for SSM Agent is available on [GitHub](https://github.com/aws/amazon-ssm-agent) so that you can adapt the agent to meet your needs\. We encourage you to submit [pull requests](https://github.com/aws/amazon-ssm-agent/blob/master/CONTRIBUTING.md) for changes that you would like to have included\. However, AWS does not currently provide support for running modified copies of this software\.

**Note**  
To view details about the different versions of SSM Agent, see the [release notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md)\.

**Topics**
+ [Manually Install SSM Agent on Amazon EC2 Linux Instances](sysman-manual-agent-install.md)
+ [Configure SSM Agent to Use a Proxy](sysman-proxy-with-ssm-agent.md)
+ [View SSM Agent Logs](sysman-agent-logs.md)
+ [Uninstall SSM Agent from Linux Instances](sysman-uninstall-agent.md)