# Configuring instance tiers<a name="systems-manager-managed-instances-tiers"></a>

This topic describes the scenarios when you must activate the advanced\-instanced tier\. 

AWS Systems Manager offers a standard\-instances tier and an advanced\-instances tier for on\-premises servers, edge devices, and virtual machines \(VMs\) in a hybrid environment\. 

You can register up to 1,000 standard hybrid nodes per account per AWS Region at no additional cost\. \(Hybrid nodes are managed nodes of any other type than Amazon Elastic Compute Cloud \(Amazon EC2\) instances\.\) However, registering more than 1,000 hybrid nodes requires that you activate the advanced\-instances tier\. There is a charge to use the advanced\-instances tier\. For more information, see [AWS Systems Manager Pricing](http://aws.amazon.com/systems-manager/pricing/)\.

Even with fewer than 1,000 registered hybrid nodes, two other scenarios require the advanced\-instances tier: 
+ You want to use Session Manager to connect to non\-EC2 nodes\.
+ You want to patch applications \(not operating systems\) released by Microsoft on non\-EC2 nodes\.
**Note**  
There is no charge to patch applications released by Microsoft on Amazon EC2 instances\.

## Advanced\-instances tier detailed scenarios<a name="systems-manager-managed-instances-tier-scenarios"></a>

The following information provides details on the three scenarios for which you must activate the advanced\-instances tier\.

Scenario 1: You want to register more than 1,000 hybrid nodes  
Using the standard\-instances tier, you can register a maximum of 1,000 non\-EC2 nodes \(on\-premises servers, edge devices, and VMs in a hybrid environment\) per AWS Region in a specific account without additional charge\. If you need to register more than 1,000 non\-EC2 nodes in a Region, you must use the advanced\-instances tier\. You can then activate as many managed servers, edge devices, and VMs in a hybrid environment as you want\. Charges for the advanced\-instances tier are based on the number of advanced instances activated as Systems Manager managed instances and the hours those instances are running\.  
All Systems Manager managed nodes that use the managed\-instance activation process described in [Create a managed\-node activation for a hybrid environment](sysman-managed-instance-activation.md) are then subject to charge if you exceed 1,000 on\-premises nodes in a Region in a specific account \.   
You can also activate existing Amazon Elastic Compute Cloud \(Amazon EC2\) instances using Systems Manager hybrid activations and work with them as non\-EC2 instances, such as for testing\. These would also qualify as hybrid nodes\. This isn't a common scenario\.

Scenario 2: Patching Microsoft\-released applications on hybrid\-activated nodes  
The advanced\-instances tier is also required if you want to patch Microsoft\-released applications on non\-EC2 nodes \(on\-premises servers, edge devices, and VMs\) in a hybrid environment\. If you activate the advanced\-instances tier to patch Microsoft applications on non\-EC2 nodes, charges are then incurred for all on\-premises nodes, even if you have fewer than 1,000\.  
There is no additional charge to patch applications released by Microsoft on Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. For more information, see [About patching applications released by Microsoft on Windows Server](patch-manager-patching-windows-applications.md)\.

Scenario 3: Connecting to hybrid\-activated nodes using Session Manager  
Session Manager provides interactive shell access to your instances\. To connect to hybrid\-activated \(non\-EC2\) nodes using Session Manager, including on\-premises servers, edge devices, and VMs in a hybrid environment, you must activate the advanced\-instances tier\. Charges are then incurred for all hybrid\-activated nodes, even if you have fewer than 1,000\.

**Summary: When do I need the advanced\-instances tier?**  
Use the following table to review when you must use the advanced\-instances tier, and for which scenarios additional charges apply\.


****  

| Scenario | Advanced\-instances tier required? | Additional charges apply? | 
| --- | --- | --- | 
|  The number of hybrid\-activated nodes in my Region in a specific account is more than 1,000\.  | Yes | Yes | 
|  I want to use Patch Manager to patch Microsoft\-released applications on any number of hybrid\-activated nodes, even less than 1,000\.  | Yes | Yes | 
|  I want to use Session Manager to connect to any number of hybrid\-activated nodes, even less than 1,000\.  | Yes | Yes | 
|  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-managed-instances-tiers.html)  | No | No | 

**Topics**
+ [Advanced\-instances tier detailed scenarios](#systems-manager-managed-instances-tier-scenarios)
+ [Turning on the advanced\-instances tier](systems-manager-managedinstances-advanced.md)
+ [Reverting from the advanced\-instances tier to the standard\-instances tier](systems-manager-managed-instances-advanced-reverting.md)