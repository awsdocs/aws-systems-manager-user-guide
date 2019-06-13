# Working with SSM Agent<a name="ssm-agent"></a>

AWS Systems Manager Agent \(SSM Agent\) is Amazon software that can be installed and configured on an Amazon EC2 instance, an on\-premises server, or a virtual machine \(VM\)\. SSM Agent makes it possible for Systems Manager to update, manage, and configure these resources\. The agent processes requests from the Systems Manager service in the AWS Cloud, and then runs them as specified in the request\. SSM Agent then sends status and execution information back to the Systems Manager service by using the Amazon Message Delivery Service \(service prefix: `ec2messages`\)\.

If you monitor traffic, you will see your Amazon EC2 instances, and any on\-premises servers or VMs in your hybrid environment, communicating with `ec2messages.*` endpoints\. For more information, see [Reference: ec2messages, ssmmessages, and Other API Calls](systems-manager-setting-up-messageAPIs.md)\. For information about porting SSM Agent logs to Amazon CloudWatch Logs, see [Monitoring AWS Systems Manager](monitoring.md)\.

**Keeping SSM Agent up\-to\-date**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate Updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

**Note**  
AMIs that include SSM Agent by default can take up to two weeks to be updated with the newest version of SSM Agent\. We recommend that you configure even more frequent automated updates to SSM Agent\.  
Updated versions of SSM Agent are rolled out to new AWS Regions at different times\. For this reason, you might receive the "Unsupported on current platform" error when trying to deploy a new version of SSM Agent in a Region\.

**About the local ssm\-user account**  
Starting with version 2\.3\.50\.0 of SSM Agent, the agent creates a local user account called `ssm-user` and adds it to `/etc/sudoers` \(Linux\) or to the Administrators group \(Windows\)\. On agent versions before 2\.3\.612\.0, the account is created the first time SSM Agent starts or restarts after installation\. On version 2\.3\.612\.0 and later, the `ssm-user` account is created the first time a session is started on an instance\. This `ssm-user` is the default OS user when a Session Manager session is started\. You can change the permissions by moving `ssm-user` to a less\-privileged group or by changing the `sudoers` file\. The `ssm-user` account is not removed from the system when SSM Agent is uninstalled\.

On Windows Server, SSM Agent handles setting a new password for the `ssm-user` account when each session starts\. No passwords are set for `ssm-user` on Linux managed instances\.

Starting with SSM Agent version 2\.3\.612\.0, the `ssm-user` account is not created automatically on Windows Server machines that are being used as domain controllers\. To use Session Manager on a Windows Server domain controller, you must create the `ssm-user` account manually if it isn't already present\.

**Important**  
In order for the ssm\-user account to be created, the instance profile attached to the instance must provide the necessary permissions\. For information, see [Verify or Create an IAM Instance Profile with Session Manager Permissions](session-manager-getting-started-instance-profile.md)\.

**AMIs with SSM Agent preinstalled**  
SSM Agent is preinstalled, by default, on the following Amazon Machine Images \(AMIs\):
+ Windows Server 2003\-2012 R2 AMIs published in November 2016 or later
+ Windows Server 2016 and 2019
+ Amazon Linux
+ Amazon Linux 2
+ Ubuntu Server 16\.04
+ Ubuntu Server 18\.04

You must manually install SSM Agent on Amazon EC2 instances created from other Linux AMIs, including non\-base images like *Amazon ECS\-Optimized AMIs*\. You must also manually install SSM Agent on on\-premises servers or VMs in your hybrid environment\. For more information, see [Setting Up AWS Systems Manager for Hybrid Environments](systems-manager-managedinstances.md)\.

**SSM Agent on GitHub**  
The source code for SSM Agent is available on [GitHub](https://github.com/aws/amazon-ssm-agent) so that you can adapt the agent to meet your needs\. We encourage you to submit [pull requests](https://github.com/aws/amazon-ssm-agent/blob/master/CONTRIBUTING.md) for changes that you would like to have included\. However, Amazon Web Services does not currently provide support for running modified copies of this software\.

**Topics**
+ [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md)
+ [Installing and Configuring SSM Agent on Amazon EC2 Linux Instances](sysman-install-ssm-agent.md)
+ [Restrict Access to Root\-Level Commands Through SSM Agent](ssm-agent-restrict-root-level-commands.md)
+ [Automate Updates to SSM Agent](ssm-agent-automatic-updates.md)
+ [Subscribe to SSM Agent Notifications](ssm-agent-subscribe-notifications.md)
+ [About Minimum S3 Bucket Permissions for SSM Agent](ssm-agent-minimum-s3-permissions.md)