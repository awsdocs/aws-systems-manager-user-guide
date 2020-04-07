# Configuring Instance Tiers<a name="systems-manager-managed-instances-tiers"></a>

AWS Systems Manager offers a standard\-instances tier and an advanced\-instances tier for servers and VMs in your hybrid environment\. The standard\-instances tier enables you to register a maximum of 1,000 on\-premises servers or VMs per AWS account per AWS Region\. If you need to register more than 1,000 on\-premises servers or VMs in a single account and Region, then use the advanced\-instances tier\. You can activate as many managed instances in a hybrid environment as you like in the advanced\-instances tier\. However, all instances configured for Systems Manager using the managed\-instance activation process described earlier in [Step 4: Create a Managed\-Instance Activation for a Hybrid Environment](sysman-managed-instance-activation.md) are made available on a pay\-per\-use basis\. This also applies to Amazon EC2 instances that use a Systems Manager on\-premises activation \(which is not a common scenario\)\.

As long as your AWS account and Region has fewer than 1,000 on\-premises instances in your hybrid environment, you can revert back to the standard\-instances tier at any time\. 

**Topics**
+ [Enabling the Advanced\-Instances Tier](systems-manager-managedinstances-advanced.md)
+ [Reverting from the Advanced\-Instances Tier to the Standard\-Instances Tier](systems-manager-managed-instances-advanced-reverting.md)