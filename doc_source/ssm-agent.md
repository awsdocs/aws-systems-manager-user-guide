# Working with SSM Agent<a name="ssm-agent"></a>

AWS Systems Manager Agent \(SSM Agent\) is Amazon software that runs on Amazon Elastic Compute Cloud \(Amazon EC2\) instances, edge devices, and on\-premises servers and virtual machines \(VMs\)\. SSM Agent makes it possible for Systems Manager to update, manage, and configure these resources\. The agent processes requests from the Systems Manager service in the AWS Cloud, and then runs them as specified in the request\. SSM Agent then sends status and execution information back to the Systems Manager service by using the Amazon Message Delivery Service \(service prefix: `ec2messages`\)\.

If you monitor traffic, you will see that your managed nodes communicate with `ec2messages.*` endpoints\. For more information, see [Reference: ec2messages, ssmmessages, and other API calls](systems-manager-setting-up-messageAPIs.md)\. For information about porting SSM Agent logs to Amazon CloudWatch Logs, see [Monitoring AWS Systems Manager](monitoring.md)\.

**Topics**
+ [SSM Agent technical reference](ssm-agent-technical-details.md)
+ [SSM Agent version 3\.0](ssm-agent-v3.md)
+ [Installing and configuring SSM Agent on EC2 instances for Linux](sysman-install-ssm-agent.md)
+ [Installing and configuring SSM Agent on EC2 instances for macOS](install-ssm-agent-macos.md)
+ [Installing and configuring SSM Agent on EC2 instances for Windows Server](sysman-install-ssm-win.md)
+ [Installing and configuring SSM Agent on edge devices](install-ssm-agent-edge-devices.md)
+ [Checking SSM Agent status and starting the agent](ssm-agent-status-and-restart.md)
+ [Checking the SSM Agent version number](ssm-agent-get-version.md)
+ [Viewing SSM Agent logs](sysman-agent-logs.md)
+ [Restricting access to root\-level commands through SSM Agent](ssm-agent-restrict-root-level-commands.md)
+ [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)
+ [Subscribing to SSM Agent notifications](ssm-agent-subscribe-notifications.md)
+ [SSM Agent communications with AWS managed S3 buckets](ssm-agent-minimum-s3-permissions.md)
+ [Troubleshooting SSM Agent](troubleshooting-ssm-agent.md)