# Setting Up AWS Systems Manager for Hybrid Environments<a name="systems-manager-managedinstances"></a>

This section describes the setup tasks that account and system administrators perform for a *hybrid environment*\. A hybrid environment includes on\-premises servers and virtual machines \(VMs\) that have been configured for use with Systems Manager, including VMs in other cloud environments\. After these steps are complete, users who have been granted permissions by the AWS account administrator can use AWS Systems Manager to configure and manage their organization's on\-premises servers and virtual machines \(VMs\)\. 

If you plan to use Systems Manager to manage Amazon Elastic Compute Cloud \(Amazon EC2\) instances, or to use both Amazon EC2 instances and your own resources in a hybrid environment, follow the steps in [Setting Up AWS Systems Manager](systems-manager-setting-up.md) first\. 

Configuring your hybrid environment for Systems Manager enables you to do the following: 
+ Create a consistent and secure way to remotely manage your hybrid workloads from one location using the same tools or scripts\.
+ Centralize access control for actions that can be performed on your servers and VMs by using AWS Identity and Access Management \(IAM\)\.
+ Centralize auditing and your view into the actions performed on your servers and VMs by recording all actions in AWS CloudTrail\.

  For information about using CloudTrail to monitor Systems Manager actions, see [Logging AWS Systems Manager API Calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)\.
+ Centralize monitoring by configuring CloudWatch Events and Amazon SNS to send notifications about service execution success\.

  For information about using CloudWatch Events to monitor Systems Manager events, see [Monitoring Systems Manager Events with Amazon CloudWatch Events](monitoring-cloudwatch-events.md)\.

**About managed instances**  
After you finish configuring your servers and VMs for Systems Manager as described in this section, your hybrid machines are listed in the AWS Management Console and described as *managed instances*\. Amazon EC2 instances configured for Systems Manager are also described as managed instances\. In the console, however, the IDs of your hybrid instances are distinguished from Amazon EC2 instances with the prefix "mi\-"\. Amazon EC2 instance IDs use the prefix "i\-"\.

**About instance tiers**  
AWS Systems Manager offers a standard\-instances tier and an advanced\-instances tier for servers and VMs in your hybrid environment\. The standard\-instances tier enables you to register a maximum of 1,000 on\-premises servers or VMs per AWS account per AWS Region\. If you need to register more than 1,000 on\-premises servers or VMs in a single account and Region, then use the advanced\-instances tier\. Advanced instances also enable you to connect to your hybrid machines by using AWS Systems Manager Session Manager\. Session Manager provides interactive shell access to your instances\. For more information, see [Step 7: \(Optional\) Enable the Advanced\-Instances Tier](systems-manager-managedinstances-advanced.md) below\.

Complete the procedures in this section to configure your hybrid servers and VMs for Systems Manager\.

**Topics**
+ [Step 1: Complete General Systems Manager Setup Steps](hybrid-setup-general.md)
+ [Step 2: Create an IAM Service Role for a Hybrid Environment](sysman-service-role.md)
+ [Step 3: Install a TLS certificate on On\-Premises Servers and VMs](hybrid-tls-certificate.md)
+ [Step 4: Create a Managed\-Instance Activation for a Hybrid Environment](sysman-managed-instance-activation.md)
+ [Step 5: Install SSM Agent for a Hybrid Environment \(Windows\)](sysman-install-managed-win.md)
+ [Step 6: Install SSM Agent for a Hybrid Environment \(Linux\)](sysman-install-managed-linux.md)
+ [Step 7: \(Optional\) Enable the Advanced\-Instances Tier](systems-manager-managedinstances-advanced.md)