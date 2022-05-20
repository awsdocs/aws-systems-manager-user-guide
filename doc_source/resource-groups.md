# About resource groups<a name="resource-groups"></a>

With Systems Manager, you can associate AWS resources by assigning *resource tags*\. You can then view operational data for these resources as a *resource group*\. Resource groups help you monitor and troubleshoot your resources\. 

For example, you can assign a resource tag of "`Operation=Standard OS Patching`" to the following resources:
+ A group of AWS IoT Greengrass core devices 
+ A group of Amazon EC2 instances
+ A group of on\-premises servers in your own facility
+ A Systems Manager patch baseline that specifies which patches to apply to your managed instances
+ An Amazon Simple Storage Service \(Amazon S3\) bucket to store patching operation log output
+ A Systems Manager *maintenance window* that specifies the schedule for the patching operation

After tagging your resources, you can view the patch status of those resources in a Systems Manager consolidated dashboard\. If a problem arises with any of the resources, you can take corrective action immediately\. 