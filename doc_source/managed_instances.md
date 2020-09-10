# AWS Systems Manager Managed Instances<a name="managed_instances"></a>

A *managed instance* is any machine configured for AWS Systems Manager\. You can configure EC2 instances or on\-premises machines in a hybrid environment as managed instances\. Systems Manager supports various distributions of Linux, including Raspberry Pi devices, and Microsoft Windows Server\.

**Note**  
In the AWS Management Console, any machine prefixed with "mi\-" is an on\-premises server or virtual machine \(VM\) managed instance\. 

AWS Systems Manager offers a standard\-instances tier and an advanced\-instances tier for servers and VMs in your hybrid environment\. The standard\-instances tier enables you to register a maximum of 1,000 servers or VMs per AWS account per AWS Region\. If you need to register more than 1,000 servers or VMs in a single account and Region, then use the advanced\-instances tier\. You can create as many instances as you like in the advanced\-instances tier, but all instances configured for Systems Manager are priced on a pay\-per\-use basis\. For more information about enabling advanced instances, see [Enabling the advanced\-instances tier](systems-manager-managedinstances-advanced.md)\. For more information about pricing, see [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\.

**Note**  
Advanced instances also enable you to connect to your hybrid machines by using AWS Systems Manager Session Manager\. Session Manager provides interactive shell access to your instances\. For more information, see [AWS Systems Manager Session Manager](session-manager.md)\.
The standard\-instances quota also applies to EC2 instances that use a Systems Manager on\-premises activation \(which is not a common scenario\)\.
Microsoft application patching is only available on EC2 instances and in the advanced\-instances tier\. To patch Microsoft applications on on\-premises servers and VMs, you must enable the advanced\-instances tier\. For more information, see [About patching applications on Windows Server](about-windows-app-patching.md)\.

If you don't see your managed instances listed in the console, then do the following:

1. Verify that the console is open in the AWS Region where you created your managed instances\. You can switch Regions by using the list in the top, right corner of the console\. 

1. Verify that your instances meet Systems Manager requirements\. For information, see [Systems Manager prerequisites](systems-manager-prereqs.md)\.

1. For servers and VMs in a hybrid environment, verify that you completed the activation process\. For more information, see [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)\.

**Note**  
Systems Manager requires accurate time references in order to perform its operations\. If your instance's date and time are not set correctly, they may not match the signature date of your API requests\. For more information, see [Use cases and best practices](systems-manager-best-practices.md)\.

**Verify Systems Manager support on an instance**  
AWS Config provides AWS Managed Rules, which are predefined, customizable rules that AWS Config uses to evaluate whether your AWS resource configurations comply with common best practices\. AWS Config Managed Rules include the [ec2\-instance\-managed\-by\-systems\-manager](https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-managed-by-ssm.html) rule\. This rule checks whether the EC2 instances in your account are managed by Systems Manager\. For more information, see [AWS Config Managed Rules](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_use-managed-rules.html)\. 

**Verify Systems Manager Prerequisites**  
For information about Systems Manager prerequisites, see [Systems Manager prerequisites](systems-manager-prereqs.md)\. For information about configuring on\-premises servers and VMs as managed instances, see [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)\.

**Increase security posture on managed instances**  
For more information increasing your security posture against unauthorized root\-level commands on your instances, see [Restricting access to root\-level commands through SSM Agent](ssm-agent-restrict-root-level-commands.md)

**Topics**
+ [Configuring instance tiers](systems-manager-managed-instances-tiers.md)
+ [Resetting passwords on managed instances](managed-instances-password-reset.md)
+ [Deregistering managed instances in a hybrid environment](systems-manager-managed-instances-advanced-deregister.md)
+ [Troubleshooting managed instances](troubleshooting-managed-instances.md)