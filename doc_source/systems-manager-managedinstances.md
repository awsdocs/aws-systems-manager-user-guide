# Setting up AWS Systems Manager for hybrid environments<a name="systems-manager-managedinstances"></a>

This section describes the setup tasks that account and system administrators perform for a *hybrid environment*\. A hybrid environment includes on\-premises servers and virtual machines \(VMs\) that have been configured for use with Systems Manager, including VMs in other cloud environments\. After these steps are complete, users who have been granted permissions by the AWS account administrator can use AWS Systems Manager to configure and manage their organization's on\-premises servers and virtual machines \(VMs\)\. 

If you plan to use Systems Manager to manage Amazon Elastic Compute Cloud \(EC2\) instances, or to use both EC2 instances and your own resources in a hybrid environment, follow the steps in [Setting up AWS Systems Manager](systems-manager-setting-up.md) first\. 

Configuring your hybrid environment for Systems Manager enables you to do the following: 
+ Create a consistent and secure way to remotely manage your hybrid workloads from one location using the same tools or scripts\.
+ Centralize access control for actions that can be performed on your servers and VMs by using AWS Identity and Access Management \(IAM\)\.
+ Centralize auditing and your view into the actions performed on your servers and VMs by recording all actions in AWS CloudTrail\.

  For information about using CloudTrail to monitor Systems Manager actions, see [Logging AWS Systems Manager API calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)\.
+ Centralize monitoring by configuring EventBridge and Amazon SNS to send notifications about service execution success\.

  For information about using EventBridge to monitor Systems Manager events, see [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md)\.

**About managed instances**  
After you finish configuring your servers and VMs for Systems Manager as described in this section, your hybrid machines are listed in the AWS Management Console and described as *managed instances*\. EC2 instances configured for Systems Manager are also described as managed instances\. In the console, however, the IDs of your hybrid instances are distinguished from EC2 instances with the prefix "mi\-"\. EC2 instance IDs use the prefix "i\-"\.

For more information, see [AWS Systems Manager Managed Instances](managed_instances.md)\.

**About instance tiers**  
AWS Systems Manager offers a standard\-instances tier and an advanced\-instances tier for servers and VMs in your hybrid environment\. The standard\-instances tier enables you to register a maximum of 1,000 on\-premises servers or VMs per AWS account per AWS Region\. If you need to register more than 1,000 on\-premises servers or VMs in a single account and Region, then use the advanced\-instances tier\. Advanced instances also enable you to connect to your hybrid machines by using AWS Systems Manager Session Manager\. Session Manager provides interactive shell access to your instances\.

For more information, see [Configuring instance tiers](systems-manager-managed-instances-tiers.md)\.

**Topics**
+ [Step 1: Complete general Systems Manager setup steps](hybrid-setup-general.md)
+ [Step 2: Create an IAM service role for a hybrid environment](sysman-service-role.md)
+ [Step 3: Install a TLS certificate on on\-premises servers and VMs](hybrid-tls-certificate.md)
+ [Step 4: Create a managed\-instance activation for a hybrid environment](sysman-managed-instance-activation.md)
+ [Step 5: Install SSM Agent for a hybrid environment \(Linux\)](sysman-install-managed-linux.md)
+ [Step 6: Install SSM Agent for a hybrid environment \(Windows\)](sysman-install-managed-win.md)