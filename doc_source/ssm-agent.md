# Installing and Configuring SSM Agent<a name="ssm-agent"></a>

AWS Systems Manager Agent \(SSM Agent\) is Amazon software that runs on your Amazon EC2 instances and your hybrid instances that are configured for Systems Manager \(hybrid instances\)\. SSM Agent processes requests from the Systems Manager service in the cloud and configures your machine as specified in the request\. SSM Agent sends status and execution information back to the Systems Manager service by using the EC2 Messaging service\. If you monitor traffic, you will see your instances communicating with ec2messages\.\* endpoints\. For more information, see [Ec2messages and Undocumented API Calls](systems-manager-setting-up.md#systems-manager-setting-up-messageAPIs)\.

SSM Agent is installed, by default, on Amazon EC2 Windows instances and Amazon Linux instances\. You must manually install the agent on other versions of Linux and hybrid instances\. 

**Note**  
The SSM Agent download and installation process for hybrid instances is different than Amazon EC2 instances\. For more information, see [Install the SSM Agent on Servers and VMs in Your Windows Hybrid Environment](systems-manager-managedinstances.md#sysman-install-managed-win)\.

Use the following procedures to install, configure, or uninstall SSM Agent\. This section also includes information about configuring SSM Agent to use a proxy\. For information about porting SSM Agent logs to Amazon CloudWatch Logs, see [Monitoring Instances with AWS Systems Manager](monitoring.md)\.


+ [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md)
+ [Installing and Configuring SSM Agent on Linux Instances](sysman-install-ssm-agent.md)