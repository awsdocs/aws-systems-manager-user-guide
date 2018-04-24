# AWS Systems Manager Managed Instances<a name="managed_instances"></a>

A *managed instance* is any machine configured for AWS Systems Manager\. You can configure Amazon EC2 instances or on\-premises machines in a hybrid environment as managed instances\. Systems Manager supports various distributions of Linux, including Raspberry Pi devices, and Microsoft Windows\.

In the console, any machine prefixed with "mi\-" is an on\-premises server or virtual machine \(VM\) managed instance\.

For information about Systems Manager prerequisites, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\. For information about configuring on\-premises servers and VMs as managed instances, see [Setting Up AWS Systems Manager in Hybrid Environments](systems-manager-managedinstances.md)\.

**Note**  
Systems Manager requires accurate time references in order to perform its operations\. If your instance's date and time are not set correctly, they may not match the signature date of your API requests\. For more information, see [Use Cases and Best Practices](systems-manager-best-practices.md)\.