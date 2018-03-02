# AWS Systems Manager Run Command<a name="execute-remote-commands"></a>

AWS Systems Manager Run Command lets you remotely and securely manage the configuration of your managed instances\. A *managed instance* is any Amazon EC2 instance or on\-premises machine in your hybrid environment that has been configured for Systems Manager\. Run Command enables you to automate common administrative tasks and perform ad hoc configuration changes at scale\. You can use Run Command from the AWS console, the AWS Command Line Interface, AWS Tools for Windows PowerShell, or the AWS SDKs\. Run Command is offered at no additional cost\.

Administrators use Run Command to perform the following types of tasks on their managed instances: install or bootstrap applications, build a deployment pipeline, capture log files when an instance is terminated from an Auto Scaling group, and join instances to a Windows domain, to name a few\.

**Getting Started**  
The following table includes information to help you get started with Run Command\.


****  

| Topic | Details | 
| --- | --- | 
|  [Tutorial: Remotely Manage Your Amazon EC2 Instances](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/tutorial_run_command.html) \(Amazon EC2 User Guide\)  |  \(Optional\) The tutorial shows you how to quickly send a command using Run Command with AWS Tools for Windows PowerShell or the AWS Command Line Interface \(AWS CLI\)\.  | 
|  [Systems Manager Prerequisites](systems-manager-setting-up.md#systems-manager-prereqs)  |  \(Required\) Verify that your instances meet the minimum requirements for Run Command, configure required roles, and install the SSM Agent\.  | 
|  [Setting Up AWS Systems Manager in Hybrid Environments](systems-manager-managedinstances.md)  |  \(Optional\) Register on\-premises servers and VMs with AWS so that you can manage them using Run Command\.  | 
|  [Executing Commands Using Systems Manager Run Command](run-command.md)  |  Learn how to execute a command from the EC2 console and how to execute commands to a fleet of managed instances\.  | 
|  [Run Command Walkthroughs](run-command-walkthroughs.md)  |  Learn how to execute commands using either AWS Tools for Windows PowerShell or the AWS CLI\.  | 

**Components and Concepts**  
As you get started with Systems Manager Run Command, you'll benefit from understanding the components and concepts of this feature\.


****  

| Component/Concept | Details | 
| --- | --- | 
| Systems Manager Documents | A Systems Manager document defines the plugins to run and the parameters to use when a command executes on a machine\. When you execute a command, you specify the Systems Manager document that Run Command uses\. Run Command includes pre\-defined documents that enable you to quickly perform common tasks on a machine\. You can also create your own Systems Manager documents\. The first time you execute a command from a new Systems Manager document, the system stores the document with your AWS account\. For more information, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\. | 
| Commands | You can configure managed instances by sending commands from your local machine\. You don't need to log on locally to configure your instances\. You can send commands using one of the following: the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), AWS Tools for Windows PowerShell, the AWS Command Line Interface \(AWS CLI\), the Systems Manager API, or Amazon SDKs\. For more information, see [Systems Manager AWS Tools for Windows PowerShell Reference](http://docs.aws.amazon.com/powershell/latest/reference/items/Amazon_Simple_Systems_Management_cmdlets.html), [Systems Manager AWS CLI Reference](http://docs.aws.amazon.com/cli/latest/reference/ssm/index.html), and the [AWS SDKs](http://aws.amazon.com/tools/#SDKs)\. | 
| SSM Agent | The SSM Agent is AWS software that you install on your EC2 instances and servers and VMs in your hybrid environment\. The agent processes Run Command requests and configures your machine as specified in the request\. For more information, see [Installing and Configuring SSM Agent on Linux Instances](sysman-install-ssm-agent.md) \(Linux\) and [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md) \(Windows\)\. | 

For information about Systems Manager limits, see [AWS Systems Manager Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_ssm)\. To increase limits, go to [AWS Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-ec2-instances) and submit a limit increase request form\.


+ [Setting Up Run Command](sysman-rc-setting-up.md)
+ [Executing Commands Using Systems Manager Run Command](run-command.md)
+ [Understanding Command Statuses](monitor-commands.md)
+ [Run Command Walkthroughs](run-command-walkthroughs.md)
+ [Troubleshooting Systems Manager Run Command](troubleshooting-remote-commands.md)

**Related Content**

+ [Configuring Access to Systems Manager](systems-manager-access.md)

+ [Installing and Configuring SSM Agent](ssm-agent.md)

+ [Configure Run Command as a CloudWatch Events Target](sysman-rc-setting-up.md#rc-cwe-target)

+  [Amazon EC2 Systems Manager API Reference](http://docs.aws.amazon.com/ssm/latest/APIReference/) 