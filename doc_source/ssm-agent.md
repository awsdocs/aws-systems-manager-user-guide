# Working with SSM Agent<a name="ssm-agent"></a>

AWS Systems Manager Agent \(SSM Agent\) is Amazon software that can be installed and configured on an EC2 instance, an on\-premises server, or a virtual machine \(VM\)\. SSM Agent makes it possible for Systems Manager to update, manage, and configure these resources\. The agent processes requests from the Systems Manager service in the AWS Cloud, and then runs them as specified in the request\. SSM Agent then sends status and execution information back to the Systems Manager service by using the Amazon Message Delivery Service \(service prefix: `ec2messages`\)\.

If you monitor traffic, you will see your Amazon Elastic Compute Cloud \(Amazon EC2\) instances, and any on\-premises servers or VMs in your hybrid environment, communicating with `ec2messages.*` endpoints\. For more information, see [Reference: ec2messages, ssmmessages, and other API calls](systems-manager-setting-up-messageAPIs.md)\. For information about porting SSM Agent logs to Amazon CloudWatch Logs, see [Monitoring AWS Systems Manager](monitoring.md)\.

**Keeping SSM Agent up\-to\-date**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

**Note**  
Amazon Machine Images \(AMIs\) that include SSM Agent by default can take up to two weeks to be updated with the newest version of SSM Agent\. We recommend that you configure even more frequent automated updates to SSM Agent\.  
Updated versions of SSM Agent are rolled out to new AWS Regions at different times\. For this reason, you might receive the "Unsupported on current platform" or "updating amazon\-ssm\-agent to an older version, please enable allow downgrade to proceed" error when trying to deploy a new version of SSM Agent in a Region\.

**SSM Agent and the Instance Metadata Service \(IMDS\)**  
Systems Manager relies on EC2 instance metadata to function correctly\. Systems Manager can access instance metadata using either version 1 or version 2 of the Instance Metadata Service \(IMDSv1 and IMDSv2\)\. For more information, see [Instance Metadata and User Data](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**SSM Agent credentials precedence**  
When SSM Agent is installed on an instance, it requires permissions in order to communicate with the Systems Manager service\. On EC2 instances, these permissions are provided in an instance profile that is attached to the instance\. On a hybrid instance, SSM Agent normally gets the needed permissions from the shared credentials file, located at `/root/.aws/credentials` \(Linux\) or `%USERPROFILE%\.aws\credentials` \(Windows\)\. The needed permissions are added to this file during the hybrid activation process\.

In rare cases, however, an instance might end up with permissions added to more than one of the locations where SSM Agent checks for permissions to run its tasks\. 

As one example, you might configure an instance to be managed by Systems Manager\. For an EC2 instance, that configuration includes attaching an instance profile\. For an on\-premises server or virtual machine \(VM\), that means credentials are provided through the hybrid activation process\. But, then you decide to also use that instance for developer or end\-user tasks and install the AWS CLI on it\. This installation results in additional permissions being added to a credentials file on the instance\.

When you run a Systems Manager command on the instance, SSM Agent might try to use credentials different from the ones you expect it to use, such as from a credentials file instead of an instance profile\. This is because SSM Agent looks for credentials in the order prescribed for the *default credential provider chain*\.

**Note**  
On Linux, SSM Agent runs as the root user\. Therefore, the environment variables and credentials file that SSM Agent looks for in this process are those of the root user only \(`/root/.aws/credentials`\)\. SSM Agent does not look at the environment variables or credentials file of any other user accounts on the instance during the search for credentials\.

The default provider chain looks for credentials in the following order:

1. Environment variables, if configured \(`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`\)\.

1. Shared credentials file \(`$HOME/.aws/credentials` for Linux or `%USERPROFILE%\.aws\credentials` for Windows\) with permissions provided by, for example, a hybrid activation or an AWS CLI installation\.

1. An AWS Identity and Access Management \(IAM\) role for tasks if an application is present that uses an Amazon Elastic Container Service \(Amazon ECS\) task definition or RunTask API operation\.

1. An instance profile attached to an Amazon EC2 instance\.

For related information, see the following topics:
+ Instance profiles for EC2 instances – [Create an IAM instance profile for Systems Manager](setup-instance-profile.md) and [Attach an IAM instance profile to an EC2 instance](setup-launch-managed-instance.md) 
+ Hybrid activations – [Create a managed\-instance activation for a hybrid environment](sysman-managed-instance-activation.md)
+ AWS CLI credentials – [Configuration and credential file settings](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) in the *AWS Command Line Interface User Guide*
+ Default credential provider chain – [Specifying Credentials](https://docs.aws.amazon.com/sdk-for-go/latest/developer-guide/configuring-sdk.html#specifying-credentials) in the *AWS SDK for Go Developer Guide*
**Note**  
This topic in the *AWS SDK for Go Developer Guide* describes the default provider chain in terms of the SDK for Go; however, the same principles apply to evaluating credentials for SSM Agent\.

**About the local ssm\-user account**  
Starting with version 2\.3\.50\.0 of SSM Agent, the agent creates a local user account called `ssm-user` and adds it to `/etc/sudoers` \(Linux\) or to the Administrators group \(Windows\)\. On agent versions before 2\.3\.612\.0, the account is created the first time SSM Agent starts or restarts after installation\. On version 2\.3\.612\.0 and later, the `ssm-user` account is created the first time a session is started on an instance\. This `ssm-user` is the default OS user when a Session Manager session is started\. You can change the permissions by moving `ssm-user` to a less\-privileged group or by changing the `sudoers` file\. The `ssm-user` account is not removed from the system when SSM Agent is uninstalled\.

On Windows Server, SSM Agent handles setting a new password for the `ssm-user` account when each session starts\. No passwords are set for `ssm-user` on Linux managed instances\.

Starting with SSM Agent version 2\.3\.612\.0, the `ssm-user` account is not created automatically on Windows Server machines that are being used as domain controllers\. To use Session Manager on a Windows Server domain controller, you must create the `ssm-user` account manually if it isn't already present\.

**Important**  
In order for the ssm\-user account to be created, the instance profile attached to the instance must provide the necessary permissions\. For information, see [Verify or create an IAM instance profile with Session Manager permissions](session-manager-getting-started-instance-profile.md)\.

**AMIs with SSM Agent preinstalled**  
SSM Agent is preinstalled, by default, on the following Amazon Machine Images \(AMIs\):
+ Windows Server 2008\-2012 R2 AMIs published in November 2016 or later
+ Windows Server 2016 and 2019
+ Amazon Linux
+ Amazon Linux 2
+ Ubuntu Server 16\.04
+ Ubuntu Server 18\.04
+ Amazon ECS\-Optimized

You must manually install SSM Agent on EC2 instances created from other Linux AMIs\. You must also manually install SSM Agent on on\-premises servers or VMs in your hybrid environment\. For more information, see [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)\.

**SSM Agent on GitHub**  
The source code for SSM Agent is available on [GitHub](https://github.com/aws/amazon-ssm-agent) so that you can adapt the agent to meet your needs\. We encourage you to submit [pull requests](https://github.com/aws/amazon-ssm-agent/blob/master/CONTRIBUTING.md) for changes that you would like to have included\. However, Amazon Web Services does not currently provide support for running modified copies of this software\.

**Topics**
+ [SSM Agent version 3](ssm-agent-v3.md)
+ [Installing and configuring SSM Agent on EC2 instances for Linux](sysman-install-ssm-agent.md)
+ [Installing and configuring SSM Agent on EC2 instances for Windows Server](sysman-install-ssm-win.md)
+ [Checking SSM Agent status and starting the agent](ssm-agent-status-and-restart.md)
+ [Checking the SSM Agent version number](ssm-agent-get-version.md)
+ [Viewing SSM Agent logs](sysman-agent-logs.md)
+ [Restricting access to root\-level commands through SSM Agent](ssm-agent-restrict-root-level-commands.md)
+ [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)
+ [Subscribing to SSM Agent notifications](ssm-agent-subscribe-notifications.md)
+ [About minimum S3 Bucket permissions for SSM Agent](ssm-agent-minimum-s3-permissions.md)
+ [Troubleshooting SSM Agent](troubleshooting-ssm-agent.md)