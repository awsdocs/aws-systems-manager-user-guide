# Installing and configuring SSM Agent on EC2 instances for Linux<a name="sysman-install-ssm-agent"></a>

SSM Agent processes Systems Manager requests and configures your machine as specified in the request\. Use the following procedures to install, configure, or uninstall SSM Agent\.

**Important**  
SSM Agent is preinstalled, by default, on the following Amazon Machine Images \(AMIs\):  
Amazon Linux
Amazon Linux 2
Ubuntu Server 16\.04
Ubuntu Server 18\.04
Amazon ECS\-Optimized
You must manually install SSM Agent on EC2 instances created from other Linux AMIs\. 

The source code for SSM Agent is available on [GitHub](https://github.com/aws/amazon-ssm-agent) so that you can adapt the agent to meet your needs\. We encourage you to submit [pull requests](https://github.com/aws/amazon-ssm-agent/blob/master/CONTRIBUTING.md) for changes that you would like to have included\. However, AWS does not currently provide support for running modified copies of this software\.

**Note**  
To view details about the different versions of SSM Agent, see the [release notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md)\.

**Topics**
+ [Manually install SSM Agent on EC2 instances for Linux](sysman-manual-agent-install.md)
+ [Configure SSM Agent to use a proxy](sysman-proxy-with-ssm-agent.md)
+ [Uninstall SSM Agent from Linux instances](sysman-uninstall-agent.md)