# Working with SSM Agent<a name="ssm-agent"></a>

AWS Systems Manager Agent \(SSM Agent\) is Amazon software that can be installed and configured on an EC2 instance, an on\-premises server, or a virtual machine \(VM\)\. SSM Agent makes it possible for Systems Manager to update, manage, and configure these resources\. The agent processes requests from the Systems Manager service in the Amazon Web Services Cloud, and then runs them as specified in the request\. SSM Agent then sends status and execution information back to the Systems Manager service by using the Amazon Message Delivery Service \(service prefix: `ec2messages`\)\.

If you monitor traffic, you will see your Amazon Elastic Compute Cloud \(Amazon EC2\) instances, and any on\-premises servers or VMs in your hybrid environment, communicating with `ec2messages.*` endpoints\. For more information, see [Reference: ec2messages, ssmmessages, and other API calls](systems-manager-setting-up-messageAPIs.md)\. For information about porting SSM Agent logs to Amazon CloudWatch Logs, see [Monitoring AWS Systems Manager](monitoring.md)\.

**Topics**
+ [SSM Agent technical reference](ssm-agent-technical-details.md)
+ [SSM Agent version 3\.0](ssm-agent-v3.md)
+ [Installing and configuring SSM Agent on EC2 instances for Linux](sysman-install-ssm-agent.md)
+ [Installing and configuring SSM Agent on EC2 instances for macOS](install-ssm-agent-macos.md)
+ [Installing and configuring SSM Agent on EC2 instances for Windows Server](sysman-install-ssm-win.md)
+ [Checking SSM Agent status and starting the agent](ssm-agent-status-and-restart.md)
+ [Checking the SSM Agent version number](ssm-agent-get-version.md)
+ [Viewing SSM Agent logs](sysman-agent-logs.md)
+ [Restricting access to root\-level commands through SSM Agent](ssm-agent-restrict-root-level-commands.md)
+ [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)
+ [Subscribing to SSM Agent notifications](ssm-agent-subscribe-notifications.md)
+ [About minimum S3 Bucket permissions for SSM Agent](ssm-agent-minimum-s3-permissions.md)
+ [Troubleshooting SSM Agent](troubleshooting-ssm-agent.md)