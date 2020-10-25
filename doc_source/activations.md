# AWS Systems Manager hybrid activations<a name="activations"></a>

To set up servers and virtual machines \(VMs\) in your hybrid environment as managed instances, you create a managed\-instance hybrid activation\. After you complete the activation, you receive an activation code and ID\. This code/ID combination functions like an Amazon EC2 access ID and secret key to provide secure access to the Systems Manager service from your managed instances\.

For information about configuring on\-premises servers and VMs as managed instances, see [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)\.

**About AWS Systems Manager Instances Tiers**

AWS Systems Manager offers a standard\-instances tier and an advanced\-instances tier for servers and VMs in your hybrid environment\. The standard\-instances tier enables you to register a maximum of 1,000 servers or VMs per AWS account per AWS Region\. If you need to register more than 1,000 servers or VMs in a single account and Region, then use the advanced\-instances tier\. You can create as many instances as you like in the advanced\-instances tier, but all instances configured for Systems Manager are priced on a pay\-per\-use basis\. For more information about enabling advanced instances, see [Enabling the advanced\-instances tier](systems-manager-managedinstances-advanced.md)\. For more information about pricing, see [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\.

**Note**  
Advanced instances also enable you to connect to your hybrid machines by using AWS Systems Manager Session Manager\. Session Manager provides interactive shell access to your instances\. For more information, see [AWS Systems Manager Session Manager](session-manager.md)\.
The standard\-instances quota also applies to EC2 instances that use a Systems Manager on\-premises activation \(which is not a common scenario\)\.
Microsoft application patching is only available on EC2 instances and in the advanced\-instances tier\. To patch Microsoft applications on on\-premises servers and VMs, you must enable the advanced\-instances tier\. For more information, see [About patching applications on Windows Server](about-windows-app-patching.md)\.