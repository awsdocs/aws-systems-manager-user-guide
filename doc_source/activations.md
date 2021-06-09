# AWS Systems Manager Hybrid Activations<a name="activations"></a>

To set up servers and virtual machines \(VMs\) in your hybrid environment as managed instances, you create a managed\-instance hybrid activation\. After you complete the activation, you receive an activation code and ID\. This code and ID combination functions like an Amazon Elastic Compute Cloud \(Amazon EC2\) access ID and secret key to provide secure access to the AWS Systems Manager service from your managed instances\.

For information about configuring on\-premises servers and VMs as managed instances, see [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)\.

**Note**  
macOS isn't supported for Systems Manager hybrid environments\.

**About Systems Manager instances tiers**

AWS Systems Manager offers a standard\-instances tier and an advanced\-instances tier for servers and VMs in your hybrid environment\. The standard\-instances tier enables you to register a maximum of 1,000 servers or VMs per AWS account per AWS Region\. If you need to register more than 1,000 servers or VMs in a single account and Region, then use the advanced\-instances tier\. You can create as many instances as you like in the advanced\-instances tier, but all instances configured for Systems Manager are priced on a pay\-per\-use basis\. For more information about enabling advanced instances, see [Enabling the advanced\-instances tier](systems-manager-managedinstances-advanced.md)\. For more information about pricing, see [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\.

**Note**  
Advanced instances also enable you to connect to your hybrid machines by using AWS Systems Manager Session Manager\. Session Manager provides interactive shell access to your instances\. For more information, see [AWS Systems Manager Session Manager](session-manager.md)\.
The standard\-instances quota also applies to EC2 instances that use a Systems Manager on\-premises activation \(which isn't a common scenario\)\.
To patch applications released by Microsoft on virtual machines \(VMs\) on\-premises instances, you must enable the advanced\-instances tier\. There is a charge to use the advanced\-instances tier\. There is no additional charge to patch applications released by Microsoft on Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. For more information, see [About patching applications released by Microsoft on Windows Server](about-windows-app-patching.md)\.