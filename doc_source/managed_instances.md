# AWS Systems Manager Managed Instances<a name="managed_instances"></a>

A *managed instance* is any machine configured for AWS Systems Manager\. You can configure Amazon EC2 instances or on\-premises machines in a hybrid environment as managed instances\. Systems Manager supports various distributions of Linux, including Raspberry Pi devices, and Microsoft Windows Server\.

**Note**  
In the AWS console, any machine prefixed with "mi\-" is an on\-premises server or virtual machine \(VM\) managed instance\. 

AWS Systems Manager offers a standard\-instances tier and an advanced\-instances tier for servers and VMs in your hybrid environment\. The standard\-instances tier enables you to register a maximum of 1,000 servers or VMs per AWS account per AWS Region\. If you need to register more than 1,000 servers or VMs in a single account and Region, then use the advanced\-instances tier\. You can create as many instances as you like in the advanced\-instances tier, but all instances configured for Systems Manager are priced on a pay\-per\-use basis\. For more information about enabling advanced instances, see [Using the Advanced\-Instances Tier](systems-manager-managedinstances-advanced.md)\. For more information about pricing, see [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\. 

**Note**  
Advanced instances also enable you to connect to your hybrid machines by using AWS Systems Manager Session Manager\. Session Manager provides interactive shell access to your instances\. For more information, see [AWS Systems Manager Session Manager](session-manager.md)\.

If you don't see your managed instances listed in the console, then do the following:

1. Verify that the console is open in the AWS Region where you created your managed instances\. You can switch Regions by using the list in the top, right corner of the console\. 

1. Verify that your instances meet Systems Manager requirements\. For information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.

1. If you created new instances in the Amazon EC2 console that include SSM Agent and an instance profile role for Systems Manager, then you might need to wait a few minutes for the instances to reach the running status and for SSM Agent to ping the Systems Manager service in the cloud\.

1. For servers and VMs in a hybrid environment, verify that you completed the activation process\. For more information, see [Setting Up AWS Systems Manager in Hybrid Environments](systems-manager-managedinstances.md)\.

**Note**  
Systems Manager requires accurate time references in order to perform its operations\. If your instance's date and time are not set correctly, they may not match the signature date of your API requests\. For more information, see [Use Cases and Best Practices](systems-manager-best-practices.md)\.

**Related Content**
+ For information about increasing your security posture against unauthorized root\-level commands on your instances, see [Restrict Access to Root\-Level Commands Through SSM Agent](ssm-agent-restrict-root-level-commands.md)
+ AWS Config provides AWS Managed Rules, which are predefined, customizable rules that AWS Config uses to evaluate whether your AWS resource configurations comply with common best practices\. AWS Config Managed Rules include the [ec2\-instance\-managed\-by\-ssm](https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-managed-by-ssm.html) rule\. This rule checks whether the Amazon EC2 instances in your account are managed by Systems Manager\. For more information, see [AWS Config Managed Rules](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_use-managed-rules.html)\. 