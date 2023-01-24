# About patch groups<a name="sysman-patch-patchgroups"></a>

**Important**  
Patch groups are not used in patching operations that are based on *patch policies*\. For information about working with patch policies, see [Introducing patch policies](patch-policies-about.md)\.

You can use a *patch group* to associate managed nodes with a specific patch baseline in Patch Manager, a capability of AWS Systems Manager\. Patch groups help ensure that you're deploying the appropriate patches, based on the associated patch baseline rules, to the correct set of nodes\. Patch groups can also help you avoid deploying patches before they have been adequately tested\. For example, you can create patch groups for different environments \(such as Development, Test, and Production\) and register each patch group to an appropriate patch baseline\. 

When you run `AWS-RunPatchBaseline`, you can target managed nodes using their ID or tags\. SSM Agent and Patch Manager then evaluate which patch baseline to use based on the patch group value that you added to the managed node\.

You create a patch group by using Amazon Elastic Compute Cloud \(Amazon EC2\) tags\. Unlike other tagging scenarios across Systems Manager, a patch group *must* be defined with the either the tag key `Patch Group` or `PatchGroup`\. The key is case\-sensitive\. You can specify any value to help you identify and target the resources in that group, for example "web servers" or "US\-EAST\-PROD", but the key must be `Patch Group` or `PatchGroup`\.

After you create a patch group and tag managed nodes, you can register the patch group with a patch baseline\. Registering the patch group with a patch baseline ensures that the nodes within the patch group use the rules defined in the associated patch baseline\. 

For more information about how to create a patch group and associate the patch group to a patch baseline, see [Working with patch groups](sysman-patch-group-tagging.md) and [Add a patch group to a patch baseline](sysman-patch-group-tagging.md#sysman-patch-group-patchbaseline)\.

To view an example of creating a patch baseline and patch groups by using the AWS Command Line Interface \(AWS CLI\), see [Walkthrough: Patch a server environment \(AWS CLI\)](sysman-patch-cliwalk.md)\. For more information about Amazon EC2 tags, see [Tag your Amazon EC2 resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) in the *Amazon EC2 User Guide for Linux Instances*\.

## How it works<a name="how-it-works-patch-groups"></a>

When the system runs the task to apply a patch baseline to a managed node, SSM Agent verifies that a patch group value is defined for the node\. If the node is assigned to a patch group, Patch Manager then verifies which patch baseline is registered to that group\. If a patch baseline is found for that group, Patch Manager notifies SSM Agent to use the associated patch baseline\. If a node isn't configured for a patch group, Patch Manager automatically notifies SSM Agent to use the currently configured default patch baseline\.

**Important**  
A managed node can only be in one patch group\.  
A patch group can be registered with only one patch baseline for each operating system type\.  
You can't apply the `Patch Group` tag \(with a space\) to an Amazon EC2 instance if the **Allow tags in instance metadata** option is enabled on the instance\. Allowing tags in instance metadata prevents tag key names from containing spaces\. If you have [allowed tags in EC2 instance metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#allow-access-to-tags-in-IMDS), you must use the tag key `PatchGroup` \(without a space\)\.

The following diagram shows a general example of the processes that Systems Manager performs when sending a Run Command task to your fleet of servers to patch using Patch Manager\. A similar process is used when a maintenance window is configured to send a command to patch using Patch Manager\.

In this example, we have three groups of EC2 instances for Windows Server with the following tags applied:


****  

| EC2 instances group | Tags | 
| --- | --- | 
|  Group 1  |  `key=OS,value=Windows` `key=PatchGroup,value=DEV`  | 
|  Group 2  |  `key=OS,value=Windows`  | 
|  Group 3  |  `key=OS,value=Windows` `key=PatchGroup,value=QA`  | 

For this example, we also have these two Windows Server patch baselines:


****  

| Patch baseline ID | Default | Associated patch group | 
| --- | --- | --- | 
|  `pb-0123456789abcdef0`  |  Yes  |  `Default`  | 
|  `pb-9876543210abcdef0`  |  No  |  `DEV`  | 

**Diagram 1: General example of patching operations process flow**

The following diagram shows how Patch Manager determines which patch baselines to use in patching operations\.

![\[Diagram showing how patch baselines are determined when performing patching operations.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/patch-groups-how-it-works.png)

The general process to scan or install patches using Run Command, a capability of AWS Systems Manager, and Patch Manager is as follows:

1. **Send a command to patch**: Use the Systems Manager console, SDK, AWS Command Line Interface \(AWS CLI\), or AWS Tools for Windows PowerShell to send a Run Command task using the document `AWS-RunPatchBaseline`\. The diagram shows a Run Command task to patch managed instances by targeting the tag `key=OS,value=Windows`\.

1. **Patch baseline determination**: SSM Agent verifies the patch group tags applied to the EC2 instance and queries Patch Manager for the corresponding patch baseline\.
   + **Matching patch group value associated with patch baseline:**

     1. SSM Agent, which is installed on EC2 instances in group one, receives the command issued in Step 1 to begin a patching operation\. SSM Agent validates that the EC2 instances have the patch group tag\-value `DEV` applied and queries Patch Manager for an associated patch baseline\.

     1. Patch Manager verifies that patch baseline `pb-9876543210abcdef0` has the patch group `DEV` associated and notifies SSM Agent\.

     1. SSM Agent retrieves a patch baseline snapshot from Patch Manager based on the approval rules and exceptions configured in `pb-9876543210abcdef0` and proceeds to the next step\.
   + **No patch group tag added to instance:**

     1. SSM Agent, which is installed on EC2 instances in group two, receives the command issued in Step 1 to begin a patching operation\. SSM Agent validates that the EC2 instances don't have a `Patch Group` or `PatchGroup` tag applied and as a result, SSM Agent queries Patch Manager for the default Windows patch baseline\.

     1. Patch Manager verifies that the default Windows Server patch baseline is `pb-0123456789abcdef0` and notifies SSM Agent\.

     1. SSM Agent retrieves a patch baseline snapshot from Patch Manager based on the approval rules and exceptions configured in the default patch baseline `pb-0123456789abcdef0` and proceeds to the next step\.
   + **No matching patch group value associated with a patch baseline:**

     1. SSM Agent, which is installed on EC2 instances in group three, receives the command issued in Step 1 to begin a patching operation\. SSM Agent validates that the EC2 instances have the patch group tag\-value `QA` applied and queries Patch Manager for an associated patch baseline\.

     1. Patch Manager doesn't find a patch baseline that has the patch group `QA` associated\.

     1. Patch Manager notifies SSM Agent to use the default Windows patch baseline `pb-0123456789abcdef0`\.

     1. SSM Agent retrieves a patch baseline snapshot from Patch Manager based on the approval rules and exceptions configured in the default patch baseline `pb-0123456789abcdef0` and proceeds to the next step\.

1. **Patch scan or install**: After determining the appropriate patch baseline to use, SSM Agent begins either scanning for or installing patches based on the operation value specified in Step 1\. The patches that are scanned for or installed are determined by the approval rules and patch exceptions defined in the patch baseline snapshot provided by Patch Manager\.

**More info**  
+ [Understanding patch compliance state values](about-patch-compliance-states.md)