# Examples: Register Targets with a Maintenance Window<a name="mw-cli-tutorial-targets-examples"></a>

You can register a single instance as a target using its instance ID, as demonstrated in [Step 2: Register a Target Instance with the Maintenance Window \(AWS CLI\)](mw-cli-tutorial-targets.md)\. You can also register one or more instances as targets using the command formats on this page\.

In general, there are two methods for identifying the instances you want to use as Maintenance Window targets: specifying individual instances, and using resource tags\. Using resource tags provides you with more options, as shown in examples 2\-4\. 

For information about limits for Maintenance Windows, in addition to those specified in the following examples, see [AWS Systems Manager Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_ssm) in the *Amazon Web Services General Reference*\.

**Topics**
+ [Example 1: Register Multiple Targets Using Instance IDs](#mw-target-example-1)
+ [Example 2: Register Targets Using Resource Tags Applied to Instances](#mw-target-example-2)
+ [Example 3: Register Targets Using a Group of Tag Keys \(Without Tag Values\)](#mw-target-example-3)
+ [Example 4: Register Targets for Patching Using the "Patch Group" Key](#mw-target-example-4)

## Example 1: Register Multiple Targets Using Instance IDs<a name="mw-target-example-1"></a>

Use the following command format to register multiple instances as targets using their instance IDs:

```
aws ssm register-target-with-maintenance-window --window-id "mw-0d89ef8464EXAMPLE" --resource-type "INSTANCE" --target "Key=InstanceIds,Values=i-026366cccbEXAMPLE,i-0471e04240EXAMPLE,i-0547226c05EXAMPLE"
```

**Recommended use**: Most useful when registering a unique group of instances with any Maintenance Window for the first time and they do *not* share a common instance tag\.

**Limits:** You can specify up to 50 instances total for each Maintenance Window target\.

## Example 2: Register Targets Using Resource Tags Applied to Instances<a name="mw-target-example-2"></a>

Use the following command format to register instances that are all already tagged with a key\-value pair you have assigned:

```
aws ssm register-target-with-maintenance-window --window-id "mw-0d89ef8464EXAMPLE" --resource-type "INSTANCE" --target "Key=tag:Region,Values=East"
```

**Recommended use**: Most useful when registering a unique group of instances with any Maintenance Window for the first time and they *do* share a common instance tag\.

**Limits:** You can specify up to five \(5\) key\-value pairs total for each target\. The total number of instances for the target represented by those tags can't exceed 50\.

## Example 3: Register Targets Using a Group of Tag Keys \(Without Tag Values\)<a name="mw-target-example-3"></a>

Use the following command to register instances that all have one or more tag keys assigned to them, regardless of their key values\.

```
aws ssm register-target-with-maintenance-window --window-id "mw-0d89ef8464EXAMPLE" --resource-type "INSTANCE" --target "Key=tag-key,Values=Name,Instance-Type,CostCenter"
```

**Recommended use**: Useful when you want to target instances by specifying multiple tag *keys* \(without their values\) rather than just one tag\-key or a tag key\-value pair\.

**Limits:** You can specify up to five \(5\) tag\-keys total for each target\. The total number of instances for the target represented by those tag\-keys can't exceed 50\.

## Example 4: Register Targets for Patching Using the "Patch Group" Key<a name="mw-target-example-4"></a>

Use the following command to register instances that you want to associate with the same patch baseline:

```
aws ssm register-target-with-maintenance-window --window-id "mw-0d89ef8464EXAMPLE" --resource-type "INSTANCE" --target "Key=tag:Patch Group,Values=my-patch-group"
```

**Recommended use**: Useful exclusively when \(1\) you tagged a group of instances with `Name=Patch Group` and `Value=patch-group-name`, and \(2\) you are using the Maintenance Window specifically for updating the instances in the patch group with the patches identified by their patch baseline\.

You create a patch group by using Amazon EC2 tags\. Unlike other tagging scenarios across Systems Manager, a patch group must be defined with the tag key name `Patch Group`, but you can specify any value for your patch group\. Each instance can be added to only one patch group\. For more information, see [About Patch Groups](sysman-patch-patchgroups.md)\.

**Limits:** You can specify up to five \(5\) key\-value pairs total for each target\. The total number of instances for the target represented by those tags can't exceed 50\.