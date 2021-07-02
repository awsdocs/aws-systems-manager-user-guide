# AWS Systems Manager State Manager<a name="systems-manager-state"></a>

**Note**  
State Manager and Maintenance Windows can perform some similar types of updates on your managed instances\. Which one you choose depends on whether you need to automate system compliance or perform high\-priority, time\-sensitive tasks during periods you specify\.  
For more information, see [Choosing between State Manager and Maintenance Windows](state-manager-vs-maintenance-windows.md)\.

State Manager, a capability of AWS Systems Manager, is a secure and scalable configuration management service that automates the process of keeping your Amazon Elastic Compute Cloud \(Amazon EC2\) and hybrid infrastructure in a state that you define\.

## How can State Manager benefit my organization?<a name="state-manager-benefits"></a>

State Manager offers these benefits:
+ Bootstrap instances with specific software at start\-up\.
+ Download and update agents on a defined schedule, including the SSM Agent\.
+ Configure network settings\.
+ Join instances to a Windows domain \(only Windows Server instances\)\.
+ Patch instances with software updates throughout their lifecycle\.
+ Run scripts on Linux, macOS, and Windows managed instances throughout their lifecycle\.

## What are the features of State Manager?<a name="state-manager-features"></a>

Key features of State Manager include the following:
+ 

**EventBridge support**  
This Systems Manager capability is supported as both an *event* type and a *target* type in Amazon EventBridge rules\. For information, see [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md) and [Reference: Amazon EventBridge event patterns and types for Systems Manager](reference-eventbridge-events.md)\.
+ **AWS CloudTrail integration**

  State Manager integrates with AWS CloudTrail to provide a record of all executions that you can audit, and Amazon EventBridge to track state changes\. You can also choose to store and view detailed command output in Amazon Simple Storage Service \(Amazon S3\)\. For more information, see the following topics:
  + [Logging AWS Systems Manager API calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)
  + [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md)
  + [\(Optional\) Set Up integrations with other AWS services](setup-integrations.md)

## What is an association?<a name="state-manager-association-what-is"></a>

A State Manager association is a configuration that is assigned to your managed instances\. The configuration defines the state that you want to maintain on your instances\. For example, an association can specify that antivirus software must be installed and running on your instances, or that certain ports must be closed\. The association specifies a schedule for when the configuration is applied once or reapplied at specified times\. The association also specifies actions to take when applying the chef configuration\. For example, an association for antivirus software might run once a day\. If the software isn't installed, then State Manager installs it\. If the software is installed, but the service isn't running, then the association might instruct State Manager to start the service\.

## Getting started with State Manager<a name="state-manager-getting-started"></a>

Complete the following tasks to get started with State Manager\.


****  

| Task | For More Information | 
| --- | --- | 
|  Verify Systems Manager prerequisites  |  [Systems Manager prerequisites](systems-manager-prereqs.md)  | 
|  Learn more about State Manager  |  [About State Manager](sysman-state-about.md)  | 
|  Create and assign a State Manager association to your instances  |  [Creating associations](sysman-state-assoc.md)  | 

**Related content**
+ [Combating Configuration Drift Using Amazon EC2 Systems Manager and Windows PowerShell DSC](http://aws.amazon.com/blogs/mt/combating-configuration-drift-using-amazon-ec2-systems-manager-and-windows-powershell-dsc/)
+ [Configure Amazon EC2 Instances in an Auto Scaling Group Using State Manager](http://aws.amazon.com/blogs/mt/configure-amazon-ec2-instances-in-an-auto-scaling-group-using-state-manager/)

**Topics**
+ [How can State Manager benefit my organization?](#state-manager-benefits)
+ [What are the features of State Manager?](#state-manager-features)
+ [What is an association?](#state-manager-association-what-is)
+ [Getting started with State Manager](#state-manager-getting-started)
+ [About State Manager](sysman-state-about.md)
+ [Working with associations in Systems Manager](systems-manager-associations.md)
+ [AWS Systems Manager State Manager walkthroughs](sysman-state-walk.md)
