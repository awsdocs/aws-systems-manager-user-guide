# AWS Systems Manager Managed Instances<a name="managed_instances"></a>

A *managed instance* is any machine configured for AWS Systems Manager\. You can configure Amazon EC2 instances or on\-premises machines in a hybrid environment as managed instances\. Systems Manager supports various distributions of Linux, including Raspberry Pi devices, and Microsoft Windows\.

In the console, any machine prefixed with "mi\-" is an on\-premises server or virtual machine \(VM\) managed instance\.

AWS Config provides AWS Managed Rules, which are predefined, customizable rules that AWS Config uses to evaluate whether your AWS resource configurations comply with common best practices\. AWS Config Managed Rules include the [ec2\-instance\-managed\-by\-ssm](https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-managed-by-ssm.html) rule\. This rule checks whether the Amazon EC2 instances in your account are managed by Systems Manager\. For more information, see [AWS Config Managed Rules](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_use-managed-rules.html)\. 

For information about Systems Manager prerequisites, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\. For information about configuring on\-premises servers and VMs as managed instances, see [Setting Up AWS Systems Manager in Hybrid Environments](systems-manager-managedinstances.md)\.

For more information increasing your security posture against unauthorized root\-level commands on your instances, see [Restrict Access to Root\-Level Commands Through SSM Agent](ssm-agent-restrict-root-level-commands.md)

**Note**  
Systems Manager requires accurate time references in order to perform its operations\. If your instance's date and time are not set correctly, they may not match the signature date of your API requests\. For more information, see [Use Cases and Best Practices](systems-manager-best-practices.md)\.