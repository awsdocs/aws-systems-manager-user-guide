# Examples: Register targets with a maintenance window<a name="mw-cli-tutorial-targets-examples"></a>

You can register a single instance as a target using its instance ID, as demonstrated in [Step 2: Register a target instance with the maintenance window \(AWS CLI\)](mw-cli-tutorial-targets.md)\. You can also register one or more instances as targets using the command formats on this page\.

In general, there are two methods for identifying the instances you want to use as maintenance window targets: specifying individual instances, and using resource tags\. The resource tags method provides more options, as shown in examples 2\-3\. 

You can also specify one or more resource groups as the target of a maintenance window\. A resource group can include instances and many other types of supported AWS resources\. Examples 4 and 5, next, demonstrate how to add resource groups to your maintenance window targets\.

For more information about creating and managing resource groups, see [What is AWS Resource Groups?](https://docs.aws.amazon.com/ARG/latest/userguide/) in the *AWS Resource Groups User Guide* and [Resource Groups and Tagging for AWS](http://aws.amazon.com/blogs/aws/resource-groups-and-tagging/) in the *AWS News Blog*\.

For information about limits for the Maintenance Windows capability, in addition to those specified in the following examples, see [Systems Manager service quotas](https://docs.aws.amazon.com/general/latest/gr/ssm.html#limits_ssm) in the *Amazon Web Services General Reference*\.

**Topics**
+ [Example 1: Register multiple targets using instance IDs](#mw-target-example-1)
+ [Example 2: Register targets using resource tags applied to instances](#mw-target-example-2)
+ [Example 3: Register targets using a group of tag keys \(without tag values\)](#mw-target-example-3)
+ [Example 4: Register targets using a resource group name](#mw-target-example-4)
+ [Example 5: Register targets by filtering resource types in a resource group](#mw-target-example-5)

## Example 1: Register multiple targets using instance IDs<a name="mw-target-example-1"></a>

Run the following command on your local machine format to register multiple instances as targets using their instance IDs:

------
#### [ Linux ]

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

**Recommended use**: Most useful when registering a unique group of instances with any maintenance window for the first time and they do *not* share a common instance tag\.

**Quotas:** You can specify up to 50 instances total for each maintenance window target\.

## Example 2: Register targets using resource tags applied to instances<a name="mw-target-example-2"></a>

Run the following command on your local machine to register instances that are all already tagged with a key\-value pair you have assigned:

------
#### [ Linux ]

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

**Recommended use**: Most useful when registering a unique group of instances with any maintenance window for the first time and they *do* share a common instance tag\.

**Quotas:** You can specify up to five key\-value pairs total for each target\.  If you specify more than one key\-value pair, an instance must be tagged with *all* the tag keys and values you specify to be included in the target group\.

**Note**  
You can tag a group of instances with the tag\-key **Patch Group** and assign the instances a common key value, such as `my-patch-group`\. Patch Manager evaluates the **Patch Group** key on instances to help determine which patch baseline applies to them\. If your task will run the `AWS-RunPatchBaseline` SSM document \(or the legacy `AWS-ApplyPatchBaseline` SSM document\), you can specify the same **Patch Group** key\-value pair when you register targets with a maintenance window\. For example: `--target "Key=tag:Patch Group,Values=my-patch-group`\. Doing so enables you to easily use a maintenance window to update patches on a group of instances that are already associated with the same patch baseline\. For more information, see [About patch groups](sysman-patch-patchgroups.md)\.

## Example 3: Register targets using a group of tag keys \(without tag values\)<a name="mw-target-example-3"></a>

Run the following command on your local machine to register instances that all have one or more tag keys assigned to them, regardless of their key values\.

------
#### [ Linux ]

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

**Recommended use**: Useful when you want to target instances by specifying multiple tag *keys* \(without their values\) rather than just one tag\-key or a tag key\-value pair\.

**Quotas:** You can specify up to five tag\-keys total for each target\.  If you specify more than one tag key, an instance must be tagged with *all* the tag keys you specify to be included in the target group\.

## Example 4: Register targets using a resource group name<a name="mw-target-example-4"></a>

Run the following command on your local machine to register a specified resource group, regardless of the type of resources it contains\. If the tasks you assign to the maintenance window do not act on a type of resource included in this resource group, the system might report an error\. Tasks for which a supported resource type is found continue to run despite these errors\.

------
#### [ Linux ]

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

Run the following command on your local machine to register only certain resource types that belong to a resource group that you specify\. With this option, even if you add a task for a resource type that belongs to the resource group, the task won’t run if you haven’t explicitly added the resource type to the filter\.

------
#### [ Linux ]

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