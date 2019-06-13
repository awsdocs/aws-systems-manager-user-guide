# Examples: Register Targets with a Maintenance Window<a name="mw-cli-tutorial-targets-examples"></a>

You can register a single instance as a target using its instance ID, as demonstrated in [Step 2: Register a Target Instance with the Maintenance Window \(AWS CLI\)](mw-cli-tutorial-targets.md)\. You can also register one or more instances as targets using the command formats on this page\.

In general, there are two methods for identifying the instances you want to use as maintenance window targets: specifying individual instances, and using resource tags\. Using resource tags provides you with more options, as shown in examples 2\-3\. 

For information about limits for the Maintenance Windows capability, in addition to those specified in the following examples, see [AWS Systems Manager Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_ssm) in the *Amazon Web Services General Reference*\.

**Topics**
+ [Example 1: Register Multiple Targets Using Instance IDs](#mw-target-example-1)
+ [Example 2: Register Targets Using Resource Tags Applied to Instances](#mw-target-example-2)
+ [Example 3: Register Targets Using a Group of Tag Keys \(Without Tag Values\)](#mw-target-example-3)

## Example 1: Register Multiple Targets Using Instance IDs<a name="mw-target-example-1"></a>

Use the following command format to register multiple instances as targets using their instance IDs:

```
aws ssm register-target-with-maintenance-window --window-id "mw-0c50858d01EXAMPLE" --resource-type "INSTANCE" --target "Key=InstanceIds,Values=i-02573cafcfEXAMPLE,i-0471e04240EXAMPLE,i-07782c72faEXAMPLE"
```

**Recommended use**: Most useful when registering a unique group of instances with any maintenance window for the first time and they do *not* share a common instance tag\.

**Limits:** You can specify up to 50 instances total for each maintenance window target\.

## Example 2: Register Targets Using Resource Tags Applied to Instances<a name="mw-target-example-2"></a>

Use the following command format to register instances that are all already tagged with a key\-value pair you have assigned:

```
aws ssm register-target-with-maintenance-window --window-id "mw-0c50858d01EXAMPLE" --resource-type "INSTANCE" --target "Key=tag:Region,Values=East"
```

**Recommended use**: Most useful when registering a unique group of instances with any maintenance window for the first time and they *do* share a common instance tag\.

**Limits:** You can specify up to five \(5\) key\-value pairs total for each target\. 

**Note**  
You can tag a group of instances with the tag\-key **Patch Group** and assign the instances a common key value, such as `my-patch-group`\. Patch Manager evaluates the **Patch Group** key on instances to help determine which patch baseline applies to them\. If your task will run the `AWS-ApplyPatchBaseline` or `AWS-RunPatchBaseline` SSM document, you can specify the same **Patch Group** key\-value pair when you register targets with a maintenance window\. For example: `--target "Key=tag:Patch Group,Values=my-patch-group`\. Doing so enables you to easily use a maintenance window to update patches on a group of instances that are already associated with the same patch baseline\. For more information, see [About Patch Groups](sysman-patch-patchgroups.md)\.

## Example 3: Register Targets Using a Group of Tag Keys \(Without Tag Values\)<a name="mw-target-example-3"></a>

Use the following command to register instances that all have one or more tag keys assigned to them, regardless of their key values\.

```
aws ssm register-target-with-maintenance-window --window-id "mw-0c50858d01EXAMPLE" --resource-type "INSTANCE" --target "Key=tag-key,Values=Name,Instance-Type,CostCenter"
```

**Recommended use**: Useful when you want to target instances by specifying multiple tag *keys* \(without their values\) rather than just one tag\-key or a tag key\-value pair\.

**Limits:** You can specify up to five \(5\) tag\-keys total for each target\. 