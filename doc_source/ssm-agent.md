# Installing and Configuring SSM Agent<a name="ssm-agent"></a>

AWS Systems Manager Agent \(SSM Agent\) is Amazon software that runs on your Amazon EC2 instances and your hybrid instances that are configured for Systems Manager \(hybrid instances\)\. SSM Agent processes requests from the Systems Manager service in the cloud and configures your machine as specified in the request\. SSM Agent sends status and execution information back to the Systems Manager service by using the EC2 Messaging service\. If you monitor traffic, you will see your instances communicating with ec2messages\.\* endpoints\. For more information, see [Reference: ec2messages, ssmmessages, and Other API Calls](systems-manager-setting-up-messageAPIs.md)\.

Starting with version 2\.3\.50\.0 of SSM Agent, the agent creates a local user account called ssm\-user and adds it to `/etc/sudoers` \(Linux\) or to the Administrators group \(Windows\) every time the agent starts\. This ssm\-user is the default OS user when a Session Manager session is started, and the password for this user is reset on every session\. You can change the permissions by moving ssm\-user to a less\-privileged group or by changing the `sudoers` file\. The ssm\-user account is not removed from the system when SSM Agent is uninstalled\.

SSM Agent is installed, by default, on the following Amazon EC2 Amazon Machine Images \(AMIs\): 
+ Windows Server \(all SKUs\)
+ Amazon Linux
+ Amazon Linux 2
+ Ubuntu Server 16\.04
+ Ubuntu Server 18\.04

You must manually install SSM Agent on Amazon EC2 instances created from other Linux AMIs\. You must also manually install SSM Agent on servers or virtual machines in your on\-premises environment\. For more information, see [Setting Up AWS Systems Manager in Hybrid Environments](systems-manager-managedinstances.md)\.

**Note**  
SSM Agent is updated whenever changes are made to Systems Manager and when new capabilities are added\. AMIs that include SSM Agent by default can take up to two weeks to be updated with the newest version of SSM Agent\. To ensure that your instances are running the newest version of SSM Agent, we recommend that you update the agent automatically whenever a new version is available using either of the following methods\.  
Use a State Manager association\. For information, see the State Manager topic [Automatically Update SSM Agent \(CLI\)](sysman-state-cli.md)\.
Use a Maintenance Window\. For information, see the Maintenance Window topics [Automatically Update SSM Agent \(AWS CLI\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/mw-walkthrough-cli.html) and [Automatically Update SSM Agent \(Console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/mw-walkthrough-console.html)\. 
If you prefer to update SSM Agent on your instances manually, you can subscribe to notifications that AWS publishes when a new version of the agent is released\. For information, see [Subscribing to SSM Agent Notifications](ssm-agent-subscribe-notifications.md)\. For information about using Run Command to manually update one or more instances with the latest version, see [Update SSM Agent by using Run Command](rc-console.md#rc-console-agentexample)\.
Updated versions of SSM Agent are rolled out to new AWS Regions at different times\. For this reason, you might receive the "Unsupported on current platform" error when trying to deploy a new version of SSM Agent in a Region\.

 For information about porting SSM Agent logs to Amazon CloudWatch Logs, see [Monitoring AWS Systems Manager](monitoring.md)\.

Use the following procedures to install, configure, or uninstall SSM Agent\. This section also includes information about configuring SSM Agent to use a proxy\.

**Topics**
+ [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md)
+ [Installing and Configuring SSM Agent on Amazon EC2 Linux Instances](sysman-install-ssm-agent.md)
+ [Restrict Access to Root\-Level Commands Through SSM Agent](ssm-agent-restrict-root-level-commands.md)
+ [Subscribing to SSM Agent Notifications](ssm-agent-subscribe-notifications.md)
+ [Minimum S3 Bucket Permissions for SSM Agent](ssm-agent-minimum-s3-permissions.md)