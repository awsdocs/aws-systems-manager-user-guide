# Setting Up AWS Systems Manager in Hybrid Environments<a name="systems-manager-managedinstances"></a>

AWS Systems Manager enables you to remotely and securely manage on\-premises servers and virtual machines \(VMs\) in your hybrid environment\. You can also manage virtual machines in other cloud environments\. Configuring your hybrid environment for Systems Manager enables you to: 
+ Create a consistent and secure way to remotely manage your hybrid workloads from one location using the same tools or scripts\.
+ Centralize access control for actions that can be performed on your servers and VMs by using AWS Identity and Access Management \(IAM\)\.
+ Centralize auditing and your view into the actions performed on your servers and VMs because all actions are recorded in AWS CloudTrail\.

  For information about using CloudTrail to monitor Systems Manager events, see [Logging AWS Systems Manager API Calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)\.
+ Centralize monitoring because you can configure CloudWatch Events and Amazon SNS to send notifications about service execution success\.

  For information about using CloudWatch Events to monitor Systems Manager events, see [Monitoring Systems Manager Events with Amazon CloudWatch Events](monitoring-cloudwatch-events.md)\.

Complete the procedures in this section to configure your hybrid servers and VMs for Systems Manager\.

**Note**  
After you finish configuring your servers and VMs for Systems Manager as described in this section, your hybrid machines are listed in the AWS Management Console and described as *managed instances*\. Amazon EC2 instances configured for Systems Manager are *also* described as managed instances\. In the console, however, your hybrid instances are distinguished from Amazon EC2 instances with the prefix "mi\-"\.
AWS Systems Manager offers a standard\-instances tier and an advanced\-instances tier for servers and VMs in your hybrid environment\. The standard\-instances tier enables you to register a maximum of 1,000 servers or VMs per AWS account per AWS Region\. If you need to register more than 1,000 servers or VMs in a single account and Region, then use the advanced\-instances tier\. Advanced instances also enable you to connect to your hybrid machines by using AWS Systems Manager Session Manager\. Session Manager provides interactive shell access to your instances\. For more information, see [Using the Advanced\-Instances Tier](systems-manager-managedinstances-advanced.md)\.

**Topics**
+ [Using the Advanced\-Instances Tier](systems-manager-managedinstances-advanced.md)
+ [Creating an IAM Service Role for a Hybrid Environment](sysman-service-role.md)
+ [Creating a Managed\-Instance Activation for a Hybrid Environment](sysman-managed-instance-activation.md)
+ [Installing SSM Agent on Servers and Virtual Machines in a Windows Hybrid Environment](sysman-install-managed-win.md)
+ [Installing SSM Agent on Servers and Virtual Machines in a Linux Hybrid Environment](sysman-install-managed-linux.md)