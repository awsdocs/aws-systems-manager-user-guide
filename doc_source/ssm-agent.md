# Installing and Configuring SSM Agent<a name="ssm-agent"></a>

AWS Systems Manager Agent \(SSM Agent\) is Amazon software that runs on your Amazon EC2 instances and your hybrid instances that are configured for Systems Manager \(hybrid instances\)\. SSM Agent processes requests from the Systems Manager service in the cloud and configures your machine as specified in the request\. SSM Agent sends status and execution information back to the Systems Manager service by using the EC2 Messaging service\. If you monitor traffic, you will see your instances communicating with ec2messages\.\* endpoints\. For more information, see [Reference: ec2messages, ssmmessages, and Other API Calls](systems-manager-setting-up-messageAPIs.md)\.

**About the local ssm\-user account**  
Starting with version 2\.3\.50\.0 of SSM Agent, the agent creates a local user account called **ssm\-user** and adds it to `/etc/sudoers` \(Linux\) or to the Administrators group \(Windows\) every time the agent starts\. This **ssm\-user** is the default OS user when a Session Manager session is started, and the password for this user is reset on every session\. You can change the permissions by moving **ssm\-user** to a less\-privileged group or by changing the `sudoers` file\. The** ssm\-user** account is not removed from the system when SSM Agent is uninstalled\.

**Important**  
In order for the ssm\-user account to be created, the instance profile attached to the instance must provide the necessary permissions\. For information, see [Step 2: Verify or Create an IAM Instance Profile with Session Manager Permissions](session-manager-getting-started-instance-profile.md) in the **Session Manager** Getting Started content\.

**AMIs with SSM Agent preinstalled**  
SSM Agent is installed, by default, on the following Amazon EC2 Amazon Machine Images \(AMIs\):
+ Windows Server \(all SKUs\)
+ Amazon Linux
+ Amazon Linux 2
+ Ubuntu Server 16\.04
+ Ubuntu Server 18\.04

You must manually install SSM Agent on Amazon EC2 instances created from other Linux AMIs\. You must also manually install SSM Agent on servers or virtual machines in your on\-premises environment\. For more information, see [Setting Up AWS Systems Manager in Hybrid Environments](systems-manager-managedinstances.md)\.

**Note**  
AMIs that include SSM Agent by default can take up to two weeks to be updated with the newest version of SSM Agent\.  
Updated versions of SSM Agent are released frequently\. New versions are released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate SSM Agent Updates on Your Instances](ssm-agent-automatic-updates.md)\.
Updated versions of SSM Agent are rolled out to new AWS Regions at different times\. For this reason, you might receive the "Unsupported on current platform" error when trying to deploy a new version of SSM Agent in a Region\.

 For information about porting SSM Agent logs to Amazon CloudWatch Logs, see [Monitoring AWS Systems Manager](monitoring.md)\.

Use the following procedures to install, configure, or uninstall SSM Agent\. This section also includes information about configuring SSM Agent to use a proxy\.

**Topics**
+ [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md)
+ [Installing and Configuring SSM Agent on Amazon EC2 Linux Instances](sysman-install-ssm-agent.md)
+ [Restrict Access to Root\-Level Commands Through SSM Agent](ssm-agent-restrict-root-level-commands.md)
+ [Automate SSM Agent Updates on Your Instances](ssm-agent-automatic-updates.md)
+ [Subscribe to SSM Agent Notifications](ssm-agent-subscribe-notifications.md)
+ [About Minimum S3 Bucket Permissions for SSM Agent](ssm-agent-minimum-s3-permissions.md)