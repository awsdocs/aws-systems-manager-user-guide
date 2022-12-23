# AWS Systems Manager Run Command<a name="run-command"></a>

Using Run Command, a capability of AWS Systems Manager, you can remotely and securely manage the configuration of your managed nodes\. A *managed node* is any Amazon Elastic Compute Cloud \(Amazon EC2\) instance, edge device, or on\-premises server or virtual machine \(VM\) in your hybrid environment that has been configured for Systems Manager\. Run Command allows you to automate common administrative tasks and perform one\-time configuration changes at scale\. You can use Run Command from the AWS Management Console, the AWS Command Line Interface \(AWS CLI\), AWS Tools for Windows PowerShell, or the AWS SDKs\. Run Command is offered at no additional cost\. To get started with Run Command, open the [Systems Manager console](https://console.aws.amazon.com/systems-manager/run-command)\. In the navigation pane, choose **Run Command**\.

Administrators use Run Command to install or bootstrap applications, build a deployment pipeline, capture log files when an instance is removed from an Auto Scaling group, join instances to a Windows domain, and more\.

**Getting Started**  
The following table includes information to help you get started with Run Command\.


****  

| Topic | Details | 
| --- | --- | 
|  [Systems Manager prerequisites](systems-manager-prereqs.md)  |  \(Required\) Verify that your managed nodes meet the minimum requirements for Run Command, configure required roles, and install the SSM Agent\.  | 
|  [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)  |  \(Optional\) Register on\-premises servers and VMs with AWS so you can manage them using Run Command\.  | 
|  [Setting up AWS Systems Manager for edge devices](systems-manager-setting-up-edge-devices.md)  |  \(Optional\) Configure edge devices so you can manage them using Run Command\.  | 
|  [AWS Systems Manager Run Command](#run-command)  |  Learn how to run a command that targets one or more managed nodes by using the AWS Management Console\.  | 
|  [Run Command walkthroughs](run-command-walkthroughs.md)  |  Learn how to run commands using either Tools for Windows PowerShell or the AWS CLI\.  | 

**EventBridge support**  
This Systems Manager capability is supported as both an *event* type and a *target* type in Amazon EventBridge rules\. For information, see [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md) and [Reference: Amazon EventBridge event patterns and types for Systems Manager](reference-eventbridge-events.md)\.

**More info**  
+ [Remotely Run Command on an EC2 Instance \(10 minute tutorial\)](http://aws.amazon.com/getting-started/hands-on/remotely-run-commands-ec2-instance-systems-manager/)
+ [Systems Manager service quotas](https://docs.aws.amazon.com/general/latest/gr/ssm.html#limits_ssm) in the *Amazon Web Services General Reference*
+ [AWS Systems Manager API Reference](https://docs.aws.amazon.com/systems-manager/latest/APIReference/) 

**Topics**
+ [Setting up Run Command](run-command-setting-up.md)
+ [Running commands on managed nodes](running-commands.md)
+ [Using exit codes in commands](run-command-handle-exit-status.md)
+ [Understanding command statuses](monitor-commands.md)
+ [Run Command walkthroughs](run-command-walkthroughs.md)
+ [Troubleshooting Systems Manager Run Command](troubleshooting-remote-commands.md)