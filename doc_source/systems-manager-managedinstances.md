# Setting Up AWS Systems Manager in Hybrid Environments<a name="systems-manager-managedinstances"></a>

AWS Systems Manager lets you remotely and securely manage on\-premises servers and virtual machines \(VMs\) in your hybrid environment\. Configuring your hybrid environment for Systems Manager provides the following benefits\. 
+ Create a consistent and secure way to remotely manage your on\-premises workloads from one location using the same tools or scripts\.
+ Centralize access control for actions that can be performed on your servers and VMs by using AWS Identity and Access Management \(IAM\)\.
+ Centralize auditing and your view into the actions performed on your servers and VMs because all actions are recorded in AWS CloudTrail\.
+ Centralize monitoring because you can configure CloudWatch Events and Amazon SNS to send notifications about service execution success\.

Complete the procedures in this section to configure your hybrid machines for Systems Manager\.

**Important**  
After you finish, your hybrid machines that are configured for Systems Manager are listed in the Amazon EC2 console and described as *managed instances*\. Amazon EC2 instances configured for Systems Manager are *also* managed instances\. In the Amazon EC2 console, however, your on\-premise instances are distinguished from Amazon EC2 instances with the prefix "mi\-"\.

**Topics**
+ [Create an IAM Service Role for a Hybrid Environment](sysman-service-role.md)
+ [Create a Managed\-Instance Activation for a Hybrid Environment](sysman-managed-instance-activation.md)
+ [Install SSM Agent on Servers and VMs in a Windows Hybrid Environment](sysman-install-managed-win.md)
+ [Install SSM Agent on Servers and VMs in a Linux Hybrid Environment](sysman-install-managed-linux.md)