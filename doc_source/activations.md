# AWS Systems Manager Hybrid Activations<a name="activations"></a>

To set up servers, edge devices, and virtual machines \(VMs\) in your hybrid environment as managed nodes, you create a managed\-node hybrid activation\. After you complete the activation, you receive an activation code and ID\. This code and ID combination functions like an Amazon Elastic Compute Cloud \(Amazon EC2\) access ID and secret key to provide secure access to the AWS Systems Manager service from your managed nodes\.

For information about configuring AWS IoT devices, non\-AWS IoT devices, on\-premises servers, and virtual machines \(VMs\) as managed nodes, see [Setting up Systems Manager for hybrid environments](systems-manager-managedinstances.md)\. For information about configuring AWS IoT Greengrass core devices for Systems Manager, see [Setting up AWS Systems Manager for edge devices](systems-manager-setting-up-edge-devices.md)\.

**Note**  
macOS isn't supported for Systems Manager hybrid environments\.

**About Systems Manager instances tiers**

AWS Systems Manager offers a standard\-instances tier and an advanced\-instances tier\. Both support servers, edge devices, and virtual machines \(VMs\) in your hybrid environment\. The standard\-instances tier allows you to register a maximum of 1,000 machines per AWS account per AWS Region\. If you need to register more than 1,000 machines in a single account and Region, then use the advanced\-instances tier\. You can create as many managed nodes as you like in the advanced\-instances tier\. All managed nodes configured for Systems Manager are priced on a pay\-per\-use basis\. For more information about enabling the advanced instances tier, see [Turning on the advanced\-instances tier](systems-manager-managedinstances-advanced.md)\. For more information about pricing, see [AWS Systems Manager Pricing](http://aws.amazon.com/systems-manager/pricing/)\.

**Note**  
Advanced instances also allow you to connect to your hybrid machines by using AWS Systems Manager Session Manager\. Session Manager provides interactive shell access to your instances\. For more information, see [AWS Systems Manager Session Manager](session-manager.md)\.
The standard\-instances quota also applies to EC2 instances that use a Systems Manager on\-premises activation \(which isn't a common scenario\)\.
To patch applications released by Microsoft on virtual machines \(VMs\) on\-premises instances, activate the advanced\-instances tier\. There is a charge to use the advanced\-instances tier\. There is no additional charge to patch applications released by Microsoft on Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. For more information, see [About patching applications released by Microsoft on Windows Server](patch-manager-patching-windows-applications.md)\.