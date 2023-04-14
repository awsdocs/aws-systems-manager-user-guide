# Run commands at scale<a name="send-commands-multiple"></a>

You can use Run Command, a capability of AWS Systems Manager, to run commands on a fleet of managed nodes by using the `targets`\. The `targets` parameter accepts a `Key,Value` combination based on tags that you specified for your managed nodes\. When you run the command, the system locates and attempts to run the command on all managed nodes that match the specified tags\. For more information about tagging managed instances, see [Tagging your AWS resources](https://docs.aws.amazon.com/tag-editor/latest/userguide/tag-editor.html) in the *Tagging AWS Resources User Guide*\. For information about tagging your managed IoT devices, see [Tag your AWS IoT Greengrass Version 2 resources](https://docs.aws.amazon.com/greengrass/v2/developerguide/tag-resources.html) in the *AWS IoT Greengrass Version 2 Developer Guide*\. 

You can also use the `targets` parameter to target a list of specific managed node IDs, as described in the next section\.

To control how commands run across hundreds or thousands of managed nodes, Run Command also includes parameters for restricting how many nodes can simultaneously process a request and how many errors can be thrown by a command before the command is canceled\.

**Topics**
+ [Targeting multiple managed nodes](#send-commands-targeting)
+ [Using rate controls](#send-commands-rate)

## Targeting multiple managed nodes<a name="send-commands-targeting"></a>

You can run a command and target managed nodes by specifying tags, AWS resource group names, or managed node IDs\. 

The following examples show the command format when using Run Command from the AWS Command Line Interface \(AWS CLI \)\. Replace each *example resource placeholder* with your own information\. Sample commands in this section are truncated using `[...]`\.

**Example 1: Targeting tags**

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --document-name document-name \
    --targets Key=tag:tag-name,Values=tag-value \
    [...]
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name document-name ^
    --targets Key=tag:tag-name,Values=tag-value ^
    [...]
```

------

**Example 2: Targeting an AWS resource group by name**

You can specify a maximum of one resource group name per command\. When you create a resource group, we recommend including `AWS::SSM:ManagedInstance` and `AWS::EC2::Instance` as resource types in your grouping criteria\. 

**Note**  
In order to send commands that target a resource group, you must have been granted AWS Identity and Access Management \(IAM\) permissions to list or view the resources that belong to that group\. For more information, see [Set up permissions](https://docs.aws.amazon.com/ARG/latest/userguide/gettingstarted-prereqs.html#gettingstarted-prereqs-permissions) in the *AWS Resource Groups User Guide*\. 

------
#### [ Linux & macOS ]

```
aws ssm send-command \    
    --document-name document-name \
    --targets Key=resource-groups:Name,Values=resource-group-name \
    [...]
```

------
#### [ Windows ]

```
aws ssm send-command ^    
    --document-name document-name ^
    --targets Key=resource-groups:Name,Values=resource-group-name ^
    [...]
```

------

**Example 3: Targeting an AWS resource group by resource type**

You can specify a maximum of five resource group types per command\. When you create a resource group, we recommend including `AWS::SSM:ManagedInstance` and `AWS::EC2::Instance` as resource types in your grouping criteria\.

**Note**  
In order to send commands that target a resource group, you must have been granted IAM permissions to list, or view, the resources that belong to that group\. For more information, see [Set up permissions](https://docs.aws.amazon.com/ARG/latest/userguide/gettingstarted-prereqs.html#gettingstarted-prereqs-permissions) in the *AWS Resource Groups User Guide*\. 

------
#### [ Linux & macOS ]

```
aws ssm send-command \    
    --document-name document-name \
    --targets Key=resource-groups:ResourceTypeFilters,Values=resource-type-1,resource-type-2 \
    [...]
```

------
#### [ Windows ]

```
aws ssm send-command ^    
    --document-name document-name ^
    --targets Key=resource-groups:ResourceTypeFilters,Values=resource-type-1,resource-type-2 ^
    [...]
```

------

**Example 4: Targeting instance IDs**

The following examples show how to target managed nodes by using the `instanceids` key with the `targets` parameter\. You can use this key to target managed AWS IoT Greengrass core devices because each device is assigned an mi\-*ID\_number*\. You can view device IDs in Fleet Manager, a capability of AWS Systems Manager\.

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --document-name document-name \
    --targets Key=instanceids,Values=instance-ID-1,instance-ID-2,instance-ID-3 \
    [...]
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name document-name ^
    --targets Key=instanceids,Values=instance-ID-1,instance-ID-2,instance-ID-3 ^
    [...]
```

------

If you tagged managed nodes for different environments using a `Key` named `Environment` and `Values` of `Development`, `Test`, `Pre-production` and `Production`, then you could send a command to all managed nodes in *one* of these environments by using the `targets` parameter with the following syntax\.

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --document-name document-name \
    --targets Key=tag:Environment,Values=Development \
    [...]
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name document-name ^
    --targets Key=tag:Environment,Values=Development ^
    [...]
```

------

You could target additional managed nodes in other environments by adding to the `Values` list\. Separate items using commas\.

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --document-name document-name \
    --targets Key=tag:Environment,Values=Development,Test,Pre-production \
    [...]
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name document-name ^
    --targets Key=tag:Environment,Values=Development,Test,Pre-production ^
    [...]
```

------

**Variation**: Refining your targets using multiple `Key` criteria

You can refine the number of targets for your command by including multiple `Key` criteria\. If you include more than one `Key` criteria, the system targets managed nodes that meet *all* of the criteria\. The following command targets all managed nodes tagged for the Finance Department *and* tagged for the database server role\.

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --document-name document-name \
    --targets Key=tag:Department,Values=Finance Key=tag:ServerRole,Values=Database \
    [...]
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name document-name ^
    --targets Key=tag:Department,Values=Finance Key=tag:ServerRole,Values=Database ^
    [...]
```

------

**Variation**: Using multiple `Key` and `Value` criteria

Expanding on the previous example, you can target multiple departments and multiple server roles by including additional items in the `Values` criteria\.

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --document-name document-name \
    --targets Key=tag:Department,Values=Finance,Marketing Key=tag:ServerRole,Values=WebServer,Database \
    [...]
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name document-name ^
    --targets Key=tag:Department,Values=Finance,Marketing Key=tag:ServerRole,Values=WebServer,Database ^
    [...]
```

------

**Variation**: Targeting tagged managed nodes using multiple `Values` criteria

If you tagged managed nodes for different environments using a `Key` named `Department` and `Values` of `Sales` and `Finance`, then you could send a command to all of the nodes in these environments by using the `targets` parameter with the following syntax\.

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --document-name document-name \
    --targets Key=tag:Department,Values=Sales,Finance \
    [...]
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name document-name ^
    --targets Key=tag:Department,Values=Sales,Finance ^
    [...]
```

------

You can specify a maximum of five keys, and five values for each key\.

If either a tag key \(the tag name\) or a tag value includes spaces, enclose the tag key or the value in quotation marks, as shown in the following examples\.

**Example**: Spaces in `Value` tag

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --document-name document-name \
    --targets Key=tag:OS,Values="Windows Server 2016 Nano" \
    [...]
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name document-name ^
    --targets Key=tag:OS,Values="Windows Server 2016 Nano" ^
    [...]
```

------

**Example**: Spaces in `tag` key and `Value`

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --document-name document-name \
    --targets Key="tag:Operating System",Values="Windows Server 2016 Nano" \
    [...]
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name document-name ^
    --targets Key="tag:Operating System",Values="Windows Server 2016 Nano" ^
    [...]
```

------

**Example**: Spaces in one item in a list of `Values`

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --document-name document-name \
    --targets Key=tag:Department,Values="Sales","Finance","Systems Mgmt" \
    [...]
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name document-name ^
    --targets Key=tag:Department,Values="Sales","Finance","Systems Mgmt" ^
    [...]
```

------

## Using rate controls<a name="send-commands-rate"></a>

You can control the rate at which commands are sent to managed nodes in a group by using *concurrency controls *and *error controls*\.

**Topics**
+ [Using concurrency controls](#send-commands-velocity)
+ [Using error controls](#send-commands-maxerrors)

### Using concurrency controls<a name="send-commands-velocity"></a>

You can control the number of managed nodes that run a command simultaneously by using the `max-concurrency` parameter \(the **Concurrency** options in the **Run a command** page\)\. You can specify either an absolute number of managed nodes, for example **10**, or a percentage of the target set, for example **10%**\. The queueing system delivers the command to a single node and waits until the system acknowledges the initial invocation before sending the command to two more nodes\. The system exponentially sends commands to more nodes until the system meets the value of `max-concurrency`\. The default for value `max-concurrency` is 50\. The following examples show you how to specify values for the `max-concurrency` parameter\.

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --document-name document-name \
    --max-concurrency 10 \
    --targets Key=tag:Environment,Values=Development \
    [...]
```

```
aws ssm send-command \
    --document-name document-name \
    --max-concurrency 10% \
    --targets Key=tag:Department,Values=Finance,Marketing Key=tag:ServerRole,Values=WebServer,Database \
    [...]
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name document-name ^
    --max-concurrency 10 ^
    --targets Key=tag:Environment,Values=Development ^
    [...]
```

```
aws ssm send-command ^
    --document-name document-name ^
    --max-concurrency 10% ^
    --targets Key=tag:Department,Values=Finance,Marketing Key=tag:ServerRole,Values=WebServer,Database ^
    [...]
```

------

### Using error controls<a name="send-commands-maxerrors"></a>

You can also control the execution of a command to hundreds or thousands of managed nodes by setting an error limit using the `max-errors` parameters \(the **Error threshold** field in the **Run a command** page\)\. The parameter specifies how many errors are allowed before the system stops sending the command to additional managed nodes\. You can specify either an absolute number of errors, for example **10**, or a percentage of the target set, for example **10%**\. If you specify **3**, for example, the system stops sending the command when the fourth error is received\. If you specify **0**, then the system stops sending the command to additional managed nodes after the first error result is returned\. If you send a command to 50 managed nodes and set `max-errors` to **10%**, then the system stops sending the command to additional nodes when the sixth error is received\.

Invocations that are already running a command when `max-errors` is reached are allowed to complete, but some of these invocations might fail as well\. If you need to ensure that there won’t be more than `max-errors` failed invocations, set `max-concurrency` to **1** so the invocations proceed one at a time\. The default for max\-errors is 0\. The following examples show you how to specify values for the `max-errors` parameter\.

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --document-name document-name \
    --max-errors 10 \
    --targets Key=tag:Database,Values=Development \
    [...]
```

```
aws ssm send-command \
    --document-name document-name \
    --max-errors 10% \
    --targets Key=tag:Environment,Values=Development \
    [...]
```

```
aws ssm send-command \
    --document-name document-name \
    --max-concurrency 1 \
    --max-errors 1 \
    --targets Key=tag:Environment,Values=Production \
    [...]
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name document-name ^
    --max-errors 10 ^
    --targets Key=tag:Database,Values=Development ^
    [...]
```

```
aws ssm send-command ^
    --document-name document-name ^
    --max-errors 10% ^
    --targets Key=tag:Environment,Values=Development ^
    [...]
```

```
aws ssm send-command ^
    --document-name document-name ^
    --max-concurrency 1 ^
    --max-errors 1 ^
    --targets Key=tag:Environment,Values=Production ^
    [...]
```

------