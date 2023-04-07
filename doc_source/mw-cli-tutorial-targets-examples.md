# Examples: Register targets with a maintenance window<a name="mw-cli-tutorial-targets-examples"></a>

You can register a single node as a target using its node ID, as demonstrated in [Step 2: Register a target node with the maintenance window \(AWS CLI\)](mw-cli-tutorial-targets.md)\. You can also register one or more nodes as targets using the command formats on this page\.

In general, there are two methods for identifying the nodes you want to use as maintenance window targets: specifying individual nodes, and using resource tags\. The resource tags method provides more options, as shown in examples 2\-3\. 

You can also specify one or more resource groups as the target of a maintenance window\. A resource group can include nodes and many other types of supported AWS resources\. Examples 4 and 5, next, demonstrate how to add resource groups to your maintenance window targets\.

**Note**  
If a single maintenance window task is registered with multiple targets, its task invocations occur sequentially and not in parallel\. If your task must run on multiple targets at the same time, register a task for each target individually and assign each task the same priority level\.

For more information about creating and managing resource groups, see [What are resource groups?](https://docs.aws.amazon.com/ARG/latest/userguide/resource-groups.html) in the *AWS Resource Groups User Guide* and [Resource Groups and Tagging for AWS](http://aws.amazon.com/blogs/aws/resource-groups-and-tagging/) in the *AWS News Blog*\.

For information about quotas for Maintenance Windows, a capability of AWS Systems Manager, in addition to those specified in the following examples, see [Systems Manager service quotas](https://docs.aws.amazon.com/general/latest/gr/ssm.html#limits_ssm) in the *Amazon Web Services General Reference*\.

## Example 1: Register multiple targets using node IDs<a name="mw-target-example-1"></a>

Run the following command on your local machine format to register multiple nodes as targets using their node IDs\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

```
aws ssm register-target-with-maintenance-window \
    --window-id "mw-0c50858d01EXAMPLE" \
    --resource-type "INSTANCE" \
    --target "Key=InstanceIds,Values=i-02573cafcfEXAMPLE,i-0471e04240EXAMPLE,i-07782c72faEXAMPLE"
```

------
#### [ Windows ]

```
aws ssm register-target-with-maintenance-window ^
    --window-id "mw-0c50858d01EXAMPLE ^
    --resource-type "INSTANCE" ^
    --target "Key=InstanceIds,Values=i-02573cafcfEXAMPLE,i-0471e04240EXAMPLE,i-07782c72faEXAMPLE
```

------

**Recommended use**: Most useful when registering a unique group of nodes with any maintenance window for the first time and they do *not* share a common node tag\.

**Quotas:** You can specify up to 50 nodes total for each maintenance window target\.

## Example 2: Register targets using resource tags applied to nodes<a name="mw-target-example-2"></a>

Run the following command on your local machine to register nodes that are all already tagged with a key\-value pair you have assigned\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

```
aws ssm register-target-with-maintenance-window \
    --window-id "mw-0c50858d01EXAMPLE" \
    --resource-type "INSTANCE" \
    --target "Key=tag:Region,Values=East"
```

------
#### [ Windows ]

```
aws ssm register-target-with-maintenance-window ^
    --window-id "mw-0c50858d01EXAMPLE" ^
    --resource-type "INSTANCE" ^
    --target "Key=tag:Region,Values=East"
```

------

**Recommended use**: Most useful when registering a unique group of nodes with any maintenance window for the first time and they *do* share a common node tag\.

**Quotas:** You can specify up to five key\-value pairs total for each target\.  If you specify more than one key\-value pair, a node must be tagged with *all* the tag keys and values you specify to be included in the target group\.

**Note**  
You can tag a group of nodes with the tag\-key `Patch Group` or `PatchGroup` and assign the nodes a common key value, such as `my-patch-group`\. \(You must use `PatchGroup`, without a space, if you have [allowed tags in EC2 instance metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#allow-access-to-tags-in-IMDS)\.\) Patch Manager, a capability of Systems Manager, evaluates the `Patch Group` or `PatchGroup` key on nodes to help determine which patch baseline applies to them\. If your task will run the `AWS-RunPatchBaseline` SSM document \(or the legacy `AWS-ApplyPatchBaseline` SSM document\), you can specify the same `Patch Group` or `PatchGroup` key\-value pair when you register targets with a maintenance window\. For example: `--target "Key=tag:PatchGroup,Values=my-patch-group`\. Doing so allows you to use a maintenance window to update patches on a group of nodes that are already associated with the same patch baseline\. For more information, see [About patch groups](patch-manager-patch-groups.md)\.

## Example 3: Register targets using a group of tag keys \(without tag values\)<a name="mw-target-example-3"></a>

Run the following command on your local machine to register nodes that all have one or more tag keys assigned to them, regardless of their key values\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

```
aws ssm register-target-with-maintenance-window \
    --window-id "mw-0c50858d01EXAMPLE" \
    --resource-type "INSTANCE" \
    --target "Key=tag-key,Values=Name,Instance-Type,CostCenter"
```

------
#### [ Windows ]

```
aws ssm register-target-with-maintenance-window ^
    --window-id "mw-0c50858d01EXAMPLE" ^
    --resource-type "INSTANCE" ^
    --target "Key=tag-key,Values=Name,Instance-Type,CostCenter"
```

------

**Recommended use**: Useful when you want to target nodes by specifying multiple tag *keys* \(without their values\) rather than just one tag\-key or a tag key\-value pair\.

**Quotas:** You can specify up to five tag\-keys total for each target\.  If you specify more than one tag key, a node must be tagged with *all* the tag keys you specify to be included in the target group\.

## Example 4: Register targets using a resource group name<a name="mw-target-example-4"></a>

Run the following command on your local machine to register a specified resource group, regardless of the type of resources it contains\. Replace *mw\-0c50858d01EXAMPLE* with your own information\. If the tasks you assign to the maintenance window don't act on a type of resource included in this resource group, the system might report an error\. Tasks for which a supported resource type is found continue to run despite these errors\.

------
#### [ Linux & macOS ]

```
aws ssm register-target-with-maintenance-window \
    --window-id "mw-0c50858d01EXAMPLE" \
    --resource-type "RESOURCE_GROUP" \
    --target "Key=resource-groups:Name,Values=MyResourceGroup"
```

------
#### [ Windows ]

```
aws ssm register-target-with-maintenance-window ^
    --window-id "mw-0c50858d01EXAMPLE" ^
    --resource-type "RESOURCE_GROUP" ^
    --target "Key=resource-groups:Name,Values=MyResourceGroup"
```

------

**Recommended use**: Useful when you want to quickly specify a resource group as a target without evaluating whether all of its resource types will be targeted by a maintenance window, or when you know that the resource group contains only the resource types that your tasks perform actions on\.

**Quotas:** You can specify only one resource group as a target\.

## Example 5: Register targets by filtering resource types in a resource group<a name="mw-target-example-5"></a>

Run the following command on your local machine to register only certain resource types that belong to a resource group that you specify\. Replace *mw\-0c50858d01EXAMPLE* with your own information\. With this option, even if you add a task for a resource type that belongs to the resource group, the task won’t run if you haven’t explicitly added the resource type to the filter\.

------
#### [ Linux & macOS ]

```
aws ssm register-target-with-maintenance-window \
    --window-id "mw-0c50858d01EXAMPLE" \
    --resource-type "RESOURCE_GROUP" \
    --target "Key=resource-groups:Name,Values=MyResourceGroup" \
    "Key=resource-groups:ResourceTypeFilters,Values=AWS::EC2::Instance,AWS::ECS::Cluster"
```

------
#### [ Windows ]

```
aws ssm register-target-with-maintenance-window ^
    --window-id "mw-0c50858d01EXAMPLE" ^
    --resource-type "RESOURCE_GROUP" ^
    --target "Key=resource-groups:Name,Values=MyResourceGroup" ^
    "Key=resource-groups:ResourceTypeFilters,Values=AWS::EC2::Instance,AWS::ECS::Cluster"
```

------

**Recommended use**: Useful when you want to maintain strict control over the types of AWS resources your maintenance window can run actions on, or when your resource group contains a large number of resource types and you want to avoid unnecessary error reports in your maintenance window logs\.

**Quotas:** You can specify only one resource group as a target\.