# Patching managed nodes on demand<a name="patch-manager-patch-now-on-demand"></a>

Using the **Patch now** option in Patch Manager, a capability of AWS Systems Manager, you can run on\-demand patching operations from the Systems Manager console\. This means you don’t have to create a schedule in order to update the compliance status of your managed nodes or to install patches on noncompliant nodes\. You also don’t need to switch the Systems Manager console between Patch Manager and Maintenance Windows, a capability of AWS Systems Manager, in order to set up or modify a scheduled patching window\.

**Patch now** is especially useful when you must apply zero\-day updates or install other critical patches on your managed nodes as soon as possible\.

**Note**  
Patching on demand is supported for a single AWS account\-AWS Region pair at a time\. It can't be used with patching operations that are based on *patch policies*\. We recommend using patch policies for keeping all your managed nodes in compliance\. For more information about working with patch policies, see [Using Quick Setup patch policies](patch-manager-policies.md)\.

**Topics**
+ [How 'Patch now' works](#patch-on-demand-how-it-works)
+ [Running 'Patch now'](#run-patch-now)

## How 'Patch now' works<a name="patch-on-demand-how-it-works"></a>

To run **Patch now**, you specify just two required settings:
+ Whether to scan for missing patches only, or to scan *and* install patches on your managed nodes
+ Which managed nodes to run the operation on

When the **Patch now** operation runs, it determines which patch baseline to use in the same way one is selected for other patching operations\. If a managed node is associated with a patch group, the patch baseline specified for that group is used\. If the managed node isn't associated with a patch group, the operation uses the patch baseline that is currently set as the default for the operating system type of the managed node\. This can be a predefined baseline, or the custom baseline you have set as the default\. For more information about patch baseline selection, see [About patch groups](patch-manager-patch-groups.md)\. 

Options you can specify for **Patch now** include choosing when, or whether, to reboot managed nodes after patching, specifying an Amazon Simple Storage Service \(Amazon S3\) bucket to store log data for the patching operation, and running Systems Manager documents \(SSM documents\) as lifecycle hooks during patching\.

### Concurrency and error thresholds for 'Patch now'<a name="patch-on-demand-concurrency"></a>

For **Patch now** operations, concurrency and error threshold options are handled by Patch Manager\. You don't need to specify how many managed nodes to patch at once, nor how many errors are permitted before the operation fails\. Patch Manager applies the concurrency and error threshold settings described in the following tables when you patch on demand\.

**Important**  
The following thresholds apply to `Scan and install` operations only\. For `Scan` operations, Patch Manager attempts to scan up to 1,000 nodes concurrently, and continue scanning until it has encountered up to 1,000 errors\.


**Concurrency: Install operations**  

| Total number of managed nodes in the **Patch now** operation | Number of managed nodes scanned or patched at a time | 
| --- | --- | 
| Fewer than 25 | 1 | 
| 25\-100 | 5% | 
| 101 to 1,000 | 8% | 
| More than 1,000 | 10% | 


**Error threshold: Install operations**  

| Total number of managed nodes in the **Patch now** operation | Number of errors permitted before the operation fails | 
| --- | --- | 
| Fewer than 25 | 1 | 
| 25\-100 | 5 | 
| 101 to 1,000 | 10 | 
| More than 1,000 | 10 | 

### Using 'Patch now' lifecycle hooks<a name="patch-on-demand-hooks"></a>

**Patch now** provides you with the ability to run SSM Command documents as lifecycle hooks during an `Install` patching operation\. You can use these hooks for tasks such as shutting down applications before patching or running health checks on your applications after patching or after a reboot\. 

For more information about using lifecycle hooks, see [About the `AWS-RunPatchBaselineWithHooks` SSM document](patch-manager-aws-runpatchbaselinewithhooks.md)\.

The following table lists the lifecycle hooks available for each of the three **Patch now** reboot options, in addition to sample uses for each hook\.


**Lifecycle hooks and sample uses**  

| Reboot option | Hook: Before installation | Hook: After installation | Hook: On exit | Hook: After scheduled reboot | 
| --- | --- | --- | --- | --- | 
| Reboot if needed |  Run an SSM document before patching begins\. Example use: Safely shut down applications before the patching process begins\.   |  Run an SSM document at the end of the patching operation and before managed node reboot\. Example use: Run operations such as installing third\-party applications before a potential reboot\.  |  Run an SSM document immediately after the patching operation is complete\. Example use: Ensure that applications are running as expected after patching\.  | Not available | 
| Do not reboot my instances | Same as above\. |  Run an SSM document at the end of the patching operation\. Example use: Ensure that applications are running as expected after patching\.  |  *Not available*   |  *Not available*   | 
| Schedule a reboot time | Same as above\. | Same as for Do not reboot my instances\. | Not available |  Run an SSM document immediately after a scheduled reboot is complete\. Example use: Ensure that applications are running as expected after the reboot\.  | 

## Running 'Patch now'<a name="run-patch-now"></a>

Use the following procedure to patch your managed nodes on demand\.

**To run 'Patch now'**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

1. On either the **AWS Systems Manager Patch Manager** page or the **Patch baselines** page, depending on which one opens, choose **Patch now**\.

1. For **Patching operation**, choose one of the following:
   + **Scan**: Patch Manager finds which patches are missing from your managed nodes but doesn't install them\. You can view the results in the **Compliance** dashboard or in other tools you use for viewing patch compliance\.
   + **Scan and install**: Patch Manager finds which patches are missing from your managed nodes and installs them\.

1. Use this step only if you chose **Scan and install** in the previous step\. For **Reboot option**, choose one of the following:
   + **Reboot if needed**: After installation, Patch Manager reboots managed nodes only if needed to complete a patch installation\.
   + **Don't reboot my instances**: After installation, Patch Manager doesn't reboot managed nodes\. You can reboot nodes manually when you choose or manage reboots outside of Patch Manager\.
   + **Schedule a reboot time**: Specify the date, time, and UTC time zone for Patch Manager to reboot your managed nodes\. After you run the **Patch now** operation, the scheduled reboot is listed as an association in State Manager with the name `AWS-PatchRebootAssociation`\.

1. For **Instances to patch**, choose one of the following:
   + **Patch all instances**: Patch Manager runs the specified operation on all managed nodes in your AWS account in the current AWS Region\.
   + **Patch only the target instances I specify**: You specify which managed nodes to target in the next step\.

1. Use this step only if you chose **Patch only the target instances I specify** in the previous step\. In the **Target selection** section, identify the nodes on which you want to run this operation by specifying tags, selecting nodes manually, or specifying a resource group\.
**Note**  
If a managed node you expect to see isn't listed, see [Troubleshooting managed node availability](troubleshooting-managed-instances.md) for troubleshooting tips\.  
If you choose to target a resource group, note that resource groups that are based on an AWS CloudFormation stack must still be tagged with the default `aws:cloudformation:stack-id` tag\. If it has been removed, Patch Manager might be unable to determine which managed nodes belong to the resource group\.

1. \(Optional\) For **Patching log storage**, if you want to create and save logs from this patching operation, select the S3 bucket for storing the logs\.

1. \(Optional\) If you want to run SSM documents as lifecycle hooks during specific points of the patching operation, do the following:
   + Choose **Use lifecycle hooks**\.
   + For each available hook, select the SSM document to run at the specified point of the operation:
     + Before installation
     + After installation
     + On exit
     + After scheduled reboot
**Note**  
The default document, `AWS-Noop`, runs no operations\.

1. Choose **Patch now**\.

   The **Association execution summary** page opens\. \(Patch now uses associations in State Manager, a capability of AWS Systems Manager, for its operations\.\) In the **Operation summary** area, you can monitor the status of scanning or patching on the managed nodes you specified\.