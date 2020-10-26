# AWS Systems Manager State Manager<a name="systems-manager-state"></a>

**Note**  
State Manager and Maintenance Windows can perform some similar types of updates on your managed instances\. Which one you choose depends on whether you need to automate system compliance or perform high\-priority, time\-sensitive tasks only during periods you specify\.  
For more information, see [Choosing between State Manager and Maintenance Windows](state-manager-vs-maintenance-windows.md)\.

AWS Systems Manager State Manager is a secure and scalable configuration management service that automates the process of keeping your Amazon EC2 and hybrid infrastructure in a state that you define\.

The following list describes the types of tasks you can perform with State Manager\.
+ Bootstrap instances with specific software at start\-up
+ Download and update agents on a defined schedule, including SSM Agent
+ Configure network settings
+ Join instances to a Windows domain \(Windows Server instances only\)\.
+ Patch instances with software updates throughout their lifecycle
+ Run scripts on Linux and Windows managed instances throughout their lifecycle

State Manager integrates with AWS CloudTrail to provide a record of all executions that you can audit, and Amazon EventBridge to track state changes\. You can also choose to store and view detailed command output in Amazon S3\. For more information, see the following topics:
+ [Logging AWS Systems Manager API calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)
+ [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md)
+ [\(Optional\) Set Up integrations with other AWS services](setup-integrations.md)

**Amazon EventBridge support**  
This Systems Manager capability is supported as an *event* type in EventBridge rules\. For information, see [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md) and [Reference: Amazon EventBridge event patterns and types for Systems Manager](reference-eventbridge-events.md)\.

**Getting started with State Manager**  
Complete the following tasks to get started with State Manager\.


****  

| Task | For More Information | 
| --- | --- | 
|  Verify Systems Manager prerequisites  |  [Systems Manager prerequisites](systems-manager-prereqs.md)  | 
|  Learn more about State Manager  |  [About State Manager](sysman-state-about.md)  | 
|  Create and assign a State Manager association to your instances  |  [Create an association](sysman-state-assoc.md)  | 

**Related content**
+ [Combating Configuration Drift Using Amazon EC2 Systems Manager and Windows PowerShell DSC](http://aws.amazon.com/blogs/mt/combating-configuration-drift-using-amazon-ec2-systems-manager-and-windows-powershell-dsc/)
+ [Configure Amazon EC2 Instances in an Auto Scaling Group Using State Manager](http://aws.amazon.com/blogs/mt/configure-amazon-ec2-instances-in-an-auto-scaling-group-using-state-manager/)

**Topics**
+ [About State Manager](sysman-state-about.md)
+ [Working with associations in Systems Manager](systems-manager-associations.md)
+ [AWS Systems Manager State Manager walkthroughs](sysman-state-walk.md)