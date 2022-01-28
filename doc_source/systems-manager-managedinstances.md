# Setting up AWS Systems Manager for hybrid environments<a name="systems-manager-managedinstances"></a>

This section describes the setup tasks that account and system administrators perform for a *hybrid environment*\. A hybrid environment includes on\-premises servers, edge devices, and virtual machines \(VMs\) that are configured for AWS Systems Manager, including VMs in other cloud environments\. After these steps are complete, users who have been granted permissions by the AWS account administrator can use Systems Manager to configure and manage their organization's on\-premises machines\. 

**Note**  
Systems Manager supports edge devices that are configured as on\-premises machines, including AWS IoT devices and non\-AWS IoT devices\. The process for setting up these types of edge devices is described here\.  
Systems Manager also supports edge devices that use AWS IoT Greengrass Core software\. The setup process and requirements for AWS IoT Greengrass core devices is different than AWS IoT and non\-AWS edge devices\. To get started with AWS IoT Greengrass devices, see [Setting up AWS Systems Manager for edge devices](systems-manager-setting-up-edge-devices.md)\.
macOS isn't supported for Systems Manager hybrid environments\.

If you plan to use Systems Manager to manage Amazon Elastic Compute Cloud \(Amazon EC2\) instances, or to use both Amazon EC2 instances and your own resources in a hybrid environment, follow the steps in [Setting up AWS Systems Manager for EC2 instances](systems-manager-setting-up-ec2.md) first\. 

Configuring your hybrid environment for Systems Manager allows you to do the following: 
+ Create a consistent and secure way to remotely manage your hybrid workloads from one location using the same tools or scripts\.
+ Centralize access control for actions that can be performed on your machines by using AWS Identity and Access Management \(IAM\)\.
+ Centralize auditing and your view into the actions performed on your machines by recording all actions in AWS CloudTrail\.

  For information about using CloudTrail to monitor Systems Manager actions, see [Logging AWS Systems Manager API calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)\.
+ Centralize monitoring by configuring Amazon EventBridge and Amazon Simple Notification Service \(Amazon SNS\) to send notifications about service execution success\.

  For information about using EventBridge to monitor Systems Manager events, see [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md)\.

**About managed nodes**  
After you finish configuring your on\-premises servers, edge devices, and VMs for Systems Manager as described in this section, your hybrid machines are listed in the AWS Management Console and described as *managed nodes*\. In the console, the IDs of your hybrid managed nodes are distinguished from Amazon EC2 instances with the prefix "mi\-"\. Amazon EC2 instance IDs use the prefix "i\-"\.

For more information, see [Managed nodes](managed_instances.md)\.

**About instance tiers**  
Systems Manager offers a standard\-instances tier and an advanced\-instances tier for managed nodes in your hybrid environment\. The standard\-instances tier allows you to register a maximum of 1,000 on\-premises machines per AWS account per AWS Region\. If you need to register more than 1,000 on\-premises machines in a single account and Region, then use the advanced\-instances tier\. Advanced instances also allow you to connect to your hybrid machines by using AWS Systems Manager Session Manager\. Session Manager provides interactive shell access to your managed nodes\.

For more information, see [Configuring instance tiers](systems-manager-managed-instances-tiers.md)\.

**Topics**
+ [Step 1: Complete general Systems Manager setup steps](hybrid-setup-general.md)
+ [Step 2: Create an IAM service role for a hybrid environment](sysman-service-role.md)
+ [Step 3: Create a managed\-instance activation for a hybrid environment](sysman-managed-instance-activation.md)
+ [Step 4: Install SSM Agent for a hybrid environment \(Linux\)](sysman-install-managed-linux.md)
+ [Step 6: Install SSM Agent for a hybrid environment \(Windows\)](sysman-install-managed-win.md)