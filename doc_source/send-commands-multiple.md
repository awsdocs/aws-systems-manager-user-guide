# Using targets and rate controls to send commands to a fleet<a name="send-commands-multiple"></a>

You can send commands to tens, hundreds, or thousands of instances by using the `targets` parameter \(the **Specify instance tags** option in the **Run a command** page in the console\)\. The `targets` parameter accepts a `Key,Value` combination based on Amazon EC2 tags that you specified for your instances\. When you run the command, the system locates and attempts to run the command on all instances that match the specified tags\. For more information about Amazon EC2 tags, see [Tagging your Amazon EC2 resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) in the *Amazon EC2 User Guide* \(content applies to Windows Server and Linux instances\)\. You can also send commands to instances that belong to an AWS resource group\. For more information about resource groups, see [What are Resource Groups?](https://docs.aws.amazon.com/ARG/latest/userguide/) in the *AWS Resource Groups User Guide*\. 

**Note**  
You can also use the `targets` parameter to target a list of specific instance IDs, as described in the next section\.

To control command execution across hundreds or thousands of instances, Run Command also includes parameters for restricting how many instances can simultaneously process a request and how many errors can be thrown by a command before the command is terminated\.

**Topics**
+ [Targeting multiple instances](#send-commands-targeting)
+ [Using rate controls](#send-commands-rate)

## Targeting multiple instances<a name="send-commands-targeting"></a>

You can run a command and target instances by specifying tags applied to managed instances, AWS resource group names, or instance IDs\. 

**Note**  
Sample commands in this section are truncated using `[...]`\. 

For use with the AWS CLI `[send\-command](https://docs.aws.amazon.com/cli/latest/reference/ssm/send-command.html)` command, the `targets` parameter supports the syntax demonstrated in the following examples:

**Example 1: Targeting tags**

------
#### [ Linux ]

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
In order to send commands that target a resource group, you must have been granted IAM permissions to list, or view, the resources that belong to that group\. For more information, see [Set Up Permissions](https://docs.aws.amazon.com/ARG/latest/userguide/gettingstarted-prereqs.html#rg-permissions) in the *AWS Resource Groups User Guide*\. 

------
#### [ Linux ]

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
In order to send commands that target a resource group, you must have been granted IAM permissions to list, or view, the resources that belong to that group\. For more information, see [Set Up Permissions](https://docs.aws.amazon.com/ARG/latest/userguide/gettingstarted-prereqs.html#rg-permissions) in the *AWS Resource Groups User Guide*\. 

------
#### [ Linux ]

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

------
#### [ Linux ]

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

If you tagged instances for different environments using a `Key` named `Environment` and `Values` of `Development`, `Test`, `Pre-production` and `Production`, then you could send a command to all of the instances in *one* of these environments by using the `targets` parameter with the following syntax:

------
#### [ Linux ]

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

You could target additional instances in other environments by adding to the `Values` list\. Separate items using commas\.

------
#### [ Linux ]

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

You can refine the number of targets for your command by including multiple `Key` criteria\. If you include more than one `Key` criteria, the system targets instances that meet *all* of the criteria\. The following command targets all instances tagged for the Finance Department *and* tagged for the database server role\.

------
#### [ Linux ]

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
#### [ Linux ]

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

**Variation**: Targeting tagged instances using multiple `Values` criteria

If you tagged instances for different environments using a `Key` named `Department` and `Values` of `Sales` and `Finance`, then you could send a command to all of the instances in these environments by using the `targets` parameter with the following syntax:

------
#### [ Linux ]

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

**Note**  
You can specify a maximum of five keys, and five values for each key\.

If either a tag key \(the tag name\) or a tag value includes spaces, then you must enclose the tag key or the value in quotation marks, as show in the following examples\.

**Example**: Spaces in `Value` tag

------
#### [ Linux ]

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
#### [ Linux ]

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
#### [ Linux ]

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

You can control the rate at which commands are sent to instances in a group by using *concurrency controls *and *error controls*\.

**Topics**
+ [Using concurrency controls](#send-commands-velocity)
+ [Using error controls](#send-commands-maxerrors)

### Using concurrency controls<a name="send-commands-velocity"></a>

You can control how many servers run the command at the same time by using the `max-concurrency` parameter \(the **Concurrecy** options in the **Run a command** page\)\. You can specify either an absolute number of instances, for example 10, or a percentage of the target set, for example 10%\. The queueing system delivers the command to a single instance and waits until the initial invocation is acknowledged before sending the command to two more instances\. The system exponentially sends commands to more instances until the value of `max-concurrency` is met\. The default for value `max-concurrency` is 50\. The following examples show you how to specify values for the `max-concurrency` parameter\.

------
#### [ Linux ]

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

You can also control the execution of a command to hundreds or thousands of instances by setting an error limit using the `max-errors` parameters \(the **Error threshold** field in the **Run a command** page\)\. The parameter specifies how many errors are allowed before the system stops sending the command to additional instances\. You can specify either an absolute number of errors, for example **10**, or a percentage of the target set, for example **10%**\. If you specify **3**, for example, the system stops sending the command when the fourth error is received\. If you specify **0**, then the system stops sending the command to additional instances after the first error result is returned\. If you send a command to 50 instances and set `max-errors` to **10%**, then the system stops sending the command to additional instances when the sixth error is received\.

Invocations that are already running a command when `max-errors` is reached are allowed to complete, but some of these invocations may fail as well\. If you need to ensure that there won’t be more than `max-errors` failed invocations, set `max-concurrency` to **1** so the invocations proceed one at a time\. The default for max\-errors is 0\. The following examples show you how to specify values for the `max-errors` parameter\.

------
#### [ Linux ]

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