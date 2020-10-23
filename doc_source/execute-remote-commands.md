# AWS Systems Manager Run Command<a name="execute-remote-commands"></a>

AWS Systems Manager Run Command lets you remotely and securely manage the configuration of your managed instances\. A *managed instance* is any EC2 instance or on\-premises machine in your hybrid environment that has been configured for Systems Manager\. Run Command enables you to automate common administrative tasks and perform ad hoc configuration changes at scale\. You can use Run Command from the AWS console, the AWS Command Line Interface, AWS Tools for Windows PowerShell, or the AWS SDKs\. Run Command is offered at no additional cost\.

Administrators use Run Command to perform the following types of tasks on their managed instances: install or bootstrap applications, build a deployment pipeline, capture log files when an instance is terminated from an Auto Scaling group, and join instances to a Windows domain, to name a few\.

**Getting Started**  
The following table includes information to help you get started with Run Command\.


****  

| Topic | Details | 
| --- | --- | 
|  [Systems Manager prerequisites](systems-manager-prereqs.md)  |  \(Required\) Verify that your instances meet the minimum requirements for Run Command, configure required roles, and install SSM Agent\.  | 
|  [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)  |  \(Optional\) Register on\-premises servers and VMs with AWS so that you can manage them using Run Command\.  | 
|  [Running commands using Systems Manager Run Command](run-command.md)  |  Learn how to run a command from the EC2 console and how to run commands to a fleet of managed instances\.  | 
|  [Run Command walkthroughs](run-command-walkthroughs.md)  |  Learn how to run commands using either AWS Tools for Windows PowerShell or the AWS CLI\.  | 

**Amazon EventBridge support**  
This Systems Manager capability is supported as both an *event* type and a *target* type in EventBridge rules\. For information, see [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md) and [Reference: Amazon EventBridge event patterns and types for Systems Manager](reference-eventbridge-events.md)\.

**Related content**
+ [Remotely Run Commands on an EC2 Instance \(10 minute tutorial\)](https://aws.amazon.com/getting-started/tutorials/remotely-run-commands-ec2-instance-systems-manager/)
+ [Systems Manager service quotas](https://docs.aws.amazon.com/general/latest/gr/ssm.html#limits_ssm) in the *Amazon Web Services General Reference*
+ [AWS Systems Manager API Reference](https://docs.aws.amazon.com/systems-manager/latest/APIReference/) 

**Topics**
+ [Setting up Run Command](sysman-rc-setting-up.md)
+ [Running commands using Systems Manager Run Command](run-command.md)
+ [Handling exit codes with scripts](command-workflow-scripts.md)
+ [Understanding command statuses](monitor-commands.md)
+ [Run Command walkthroughs](run-command-walkthroughs.md)
+ [Troubleshooting Systems Manager Run Command](troubleshooting-remote-commands.md)