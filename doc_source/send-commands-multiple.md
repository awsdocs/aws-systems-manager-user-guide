# Sending Commands to a Fleet<a name="send-commands-multiple"></a>

You can send commands to tens, hundreds, or thousands of instances by using the `targets` parameter \(the **Select Targets by Specifying a Tag** option in the Amazon EC2 console\)\. The `targets` parameter accepts a `Key,Value` combination based on Amazon EC2 tags that you specified for your instances\. When you run the command, the system locates and attempts to run the command on all instances that match the specified tags\. For more information about Amazon EC2 tags, see [Tagging Your Amazon EC2 Resources](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) in the *Amazon EC2 User Guide* \(content applies to Windows and Linux instances\)\.

**Note**  
You can also use the `targets` parameter to target a list of specific instance IDs, as described in the next section\.

To control command execution across hundreds or thousands of instances, Run Command also includes parameters for restricting how many instances can simultaneously process a request and how many errors can be thrown by a command before the command is terminated\.

**Topics**
+ [Targeting Multiple Instances](#send-commands-targeting)
+ [Using Concurrency Controls](#send-commands-velocity)
+ [Using Error Controls](#send-commands-maxerrors)

## Targeting Multiple Instances<a name="send-commands-targeting"></a>

You can run a command and target instances by specifying either Amazon EC2 tags or instance IDs\. The `targets` parameter uses the following syntax in the AWS CLI:

Example 1: Targeting Tags

```
aws ssm send-command --document-name name --targets Key=tag:tag_name,Values=tag_value [...]
```

**Note**  
Example commands in this section are truncated using \[\.\.\.\]\.

Example 2: Targeting Instance IDs

```
aws ssm send-command --document-name name --targets Key=instanceids,Values=ID1,ID2,ID3 [...]
```

If you tagged instances for different environments using a `Key` named `Environment` and `Values` of `Development`, `Test`, `Pre-production` and `Production`, then you could send a command to all of the instances in *one* of these environments by using the `targets` parameter with the following syntax:

```
aws ssm send-command --document-name name --targets Key=tag:Environment,Values=Development [...]
```

You could target additional instances in other environments by adding to the `Values` list\. Separate items using commas\.

```
aws ssm send-command - -document-name name --targets Key=tag:Environment,Values=Development,Test,Pre-production [...]
```

Example: Refining your targets using multiple `Key` criteria

You can refine the number of targets for your command by including multiple `Key` criteria\. If you include more than one `Key` criteria, the system targets instances that meet *all* of the criteria\. The following command targets all instances tagged for the Finance Department *and* tagged for the database server role\.

```
aws ssm send-command --document-name name --targets Key=tag:Department,Values=Finance Key=tag:ServerRole,Values=Database [...]
```

Example: Using multiple `Key` and `Value` criteria

Expanding on the previous example, you can target multiple departments and multiple server roles by including additional items in the `Values` criteria\.

```
aws ssm send-command --document-name name --targets Key=tag:Department,Values=Finance,Marketing Key=tag:ServerRole,Values=WebServer,Database [...]
```

Example: Targeting tagged instances using multiple`Values` criteria

If you tagged instances for different environments using a `Key` named `Department` and `Values` of `Sales` and `Finance`, then you could send a command to all of the instances in these environments by using the `targets` parameter with the following syntax:

```
aws ssm send-command --document-name name --targets Key=tag:Department,Values=Sales,Finance [...]
```

**Note**  
You can specify a maximum of 5 keys, and 5 values for each key\.

If either a tag key \(the tag name\) or a tag value includes spaces, then you must enclose the tag key or the value in quotation marks, as show in the following examples\.

Example 1: Spaces in `Value` tag\.

```
aws ssm send-command --document-name name --targets Key=tag:OS,Values="Windows Server 2016 Nano" [...]
```

Example 2: Spaces in `tag` key and `Value`\.

```
aws ssm send-command --document-name name --targets Key="tag:Operating System",Values="Windows Server 2016 Nano" [...]
```

Example 3: Spaces in one item in a list of `Values`\.

```
aws ssm send-command --document-name name --targets Key=tag:Department,Values="Sales","Finance","Systems Mgmt" [...]
```

## Using Concurrency Controls<a name="send-commands-velocity"></a>

You can control how many servers run the command at the same time by using the `max-concurrency` parameter \(the **Execute on** field in the Amazon EC2 console\)\. You can specify either an absolute number of instances, for example 10, or a percentage of the target set, for example 10%\. The queueing system delivers the command to a single instance and waits until the initial invocation completes before sending the command to two more instances\. The system exponentially sends commands to more instances until the value of `max-concurrency` is met\. The default for value `max-concurrency` is 50\. The following examples show you how to specify values for the `max-concurrency` parameter:

```
aws ssm send-command --document-name name --max-concurrency 10 --targets Key=tag:Environment,Values=Development [...]
```

```
aws ssm send-command --document-name name --max-concurrency 10% --targets Key=tag:Department,Values=Finance,Marketing Key=tag:ServerRole,Values=WebServer,Database [...]
```

## Using Error Controls<a name="send-commands-maxerrors"></a>

You can also control the execution of a command to hundreds or thousands of instances by setting an error limit using the `max-errors` parameters \(the **Stop after \_\_ errors** field in the Amazon EC2 console\)\. The parameter specifies how many errors are allowed before the system stops sending the command to additional instances\. You can specify either an absolute number of errors, for example 10, or a percentage of the target set, for example 10%\. If you specify 3, for example, the system stops sending the command when the fourth error is received\. If you specify 0, then the system stops sending the command to additional instances after the first error result is returned\. If you send a command to 50 instances and set `max-errors` to 10%, then the system stops sending the command to additional instances when the sixth error is received\.

Invocations that are already running a command when `max-errors` is reached are allowed to complete, but some of these invocations may fail as well\. If you need to ensure that there won’t be more than `max-errors` failed invocations, set `max-concurrency` to 1 so the invocations proceed one at a time\. The default for max\-errors is 0\. The following examples show you how to specify values for the `max-errors` parameter:

```
aws ssm send-command --document-name name --max-errors 10 --targets Key=tag:Database,Values=Development [...]
```

```
aws ssm send-command --document-name name --max-errors 10% --targets Key=tag:Environment,Values=Development [...]
```

```
aws ssm send-command --document-name name --max-concurrency 1 --max-errors 1 --targets Key=tag:Environment,Values=Production [...]
```