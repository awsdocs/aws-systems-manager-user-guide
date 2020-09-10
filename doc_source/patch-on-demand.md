# Patching instances on demand<a name="patch-on-demand"></a>

Using the **Patch now** option in Patch Manager, you can run on\-demand patching operations from the Systems Manager console\. This means you don’t have to create a schedule in order to update the compliance status of your instances or to install patches on noncompliant instances\. You also don’t need to switch between the Patch Manager and Maintenance Windows areas of the Systems Manager console in order to set up or modify a scheduled patching window\.

## How 'Patch now' works<a name="patch-on-demand-how-it-works"></a>

To run **Patch now**, you specify just two options:
+ Whether to scan for missing patches only, or to scan *and* install patches on your instances
+ Which instances to run the operation on

When the **Patch now** operation runs, it uses the patch baseline that is currently set as the default for the operating system type of your instances\. \(This can be a predefined AWS baseline, or the custom baseline you have set as the default\.\) In addition, the concurrency and error threshold options are handled by Patch Manager\. You don't need to specify how many instances to patch at once, nor how many errors are permitted before the operation fails\. Patch Manager applies the concurrency and error threshold settings described in the following tables when you patch on demand\.


**Concurrency**  

| Total number of instances in the **Patch now** operation | Number of instances scanned or patched at a time | 
| --- | --- | 
| Fewer than 25 | 1 | 
| 25\-100 | 5% | 
| 101 to 1,000 | 8% | 
| More than 1,000 | 10% | 


**Error threshold**  

| Total number of instances in the **Patch now** operation | Number of errors permitted before the operation fails | 
| --- | --- | 
| Fewer than 25 | 1 | 
| 25\-100 | 5 | 
| 101 to 1,000 | 10 | 
| More than 1,000 | 10 | 

## Running 'Patch now'<a name="run-patch-now"></a>

Use the following procedure to patch your instances on demand\.

**To run 'Patch now'**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

1. On either the **AWS Systems Manager Patch Manager** page or the **Patch baselines** page, depending on which one opens, choose **Patch now**\.

1. For **Patching operation**, choose one of the following:
   + **Scan**: Patch Manager finds which patches are missing from your instances but doesn't install them\. You can view the results in the **Compliance** dashboard or in other tools you use for viewing patch compliance\.
   + **Scan and install**: Patch Manager finds which patches are missing from your instances and installs them\.

1. Only if you chose **Scan and install** in the previous step: For **Reboot option**, specify whether Patch Manager may reboot your instances now if they require it\.

1. For **Instances to patch**, choose one of the following:
   + **Patch all instances**: Patch Manager runs the specified operation on all managed instances in your account in the current AWS Region\.
   + **Patch only the target instances I specify**: You specify which instances to target in the next step\.

1. Only if you chose **Patch only the target instances I specify** in the previous step: In the **Target selection** section, identify the instances on which you want to run this operation by specifying tags, selecting instances manually, or specifying a resource group\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Some of my instances are missing](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.   
If you choose to target a resource group, note that resource groups that are based on an AWS CloudFormation stack must still be tagged with the default `aws:cloudformation:stack-id` tag\. If it has been removed, Patch Manager might be unable to determine which instances belong to the resource group\.

1. Choose **Patch now**\.

   The **Association execution summary** page opens\. \(Patch now uses State Manager associations for its operations\.\) In the** Operation summary** area, you can monitor the status of scanning or patching on the instances you specified\.