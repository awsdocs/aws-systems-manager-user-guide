# AWS Systems Manager Activations<a name="activations"></a>

To set up servers and virtual machines \(VMs\) in your hybrid environment as managed instances, you create a managed\-instance activation\. After you complete the activation, you receive an activation code and ID\. This code/ID combination functions like an Amazon EC2 access ID and secret key to provide secure access to the Systems Manager service from your managed instances\. For information about configuring on\-premises servers and VMs as managed instances, see [Setting Up AWS Systems Manager in Hybrid Environments](systems-manager-managedinstances.md)\.

**About AWS Systems Manager Instances Tiers**

AWS Systems Manager offers a standard\-instances tier and an advanced\-instances tier for servers and VMs in your hybrid environment\. The standard\-instances tier enables you to register a maximum of 1,000 servers or VMs per AWS account per AWS Region\. If you need to register more than 1,000 servers or VMs in a single account and Region, then use the advanced\-instances tier\. You can create as many instances as you like in the advanced\-instances tier, but all instances configured for Systems Manager are priced on a pay\-per\-use basis\. For more information about enabling advanced instances, see [Using the Advanced\-Instances Tier](systems-manager-managedinstances-advanced.md)\. For more information about pricing, see [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\. 

**Note**  
Advanced instances also enable you to connect to your hybrid machines by using AWS Systems Manager Session Manager\. Session Manager provides interactive shell access to your instances\. For more information, see [AWS Systems Manager Session Manager](session-manager.md)\.