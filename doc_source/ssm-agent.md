# Installing and Configuring SSM Agent<a name="ssm-agent"></a>

AWS Systems Manager Agent \(SSM Agent\) is Amazon software that runs on your Amazon EC2 instances and your hybrid instances that are configured for Systems Manager \(hybrid instances\)\. SSM Agent processes requests from the Systems Manager service in the cloud and configures your machine as specified in the request\. SSM Agent sends status and execution information back to the Systems Manager service by using the EC2 Messaging service\. If you monitor traffic, you will see your instances communicating with ec2messages\.\* endpoints\. For more information, see [Reference: Ec2messages and Other API Calls](systems-manager-setting-up-messageAPIs.md)\.

SSM Agent is installed, by default, on the following Amazon EC2 Amazon Machine Image \(AMI's\): 
+ Windows Server \(all SKUs\)
+ Amazon Linux
+ Amazon Linux 2
+ Ubuntu Server 16\.04
+ Ubuntu Server 18\.04

You must manually install the agent on Amazon EC2 instances created from other Linux AMI's and on Linux servers or virtual machines in your on\-premises environment\. 

**Note**  
The SSM Agent download and installation process for hybrid instances is different than Amazon EC2 instances\. For more information, see [Install SSM Agent on Servers and VMs in a Windows Hybrid Environment](sysman-install-managed-win.md)\.

 For information about porting SSM Agent logs to Amazon CloudWatch Logs, see [Monitoring Instances with AWS Systems Manager](monitoring.md)\.

Use the following procedures to install, configure, or uninstall SSM Agent\. This section also includes information about configuring SSM Agent to use a proxy\.

**Topics**
+ [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md)
+ [Installing and Configuring SSM Agent on Linux Instances](sysman-install-ssm-agent.md)
+ [Subscribing to SSM Agent Notifications](ssm-agent-subscribe-notifications.md)
+ [Minimum S3 Bucket Permissions for SSM Agent](ssm-agent-minimum-s3-permissions.md)