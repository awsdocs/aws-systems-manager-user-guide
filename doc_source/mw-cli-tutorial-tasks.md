# Step 3: Register a task with the maintenance window \(AWS CLI\)<a name="mw-cli-tutorial-tasks"></a>

In this step of the tutorial, you register a Run Command task that runs the `df` command on your EC2 instance for Linux\. The results of this standard Linux command show how much space is free and how much is used on the disk file system of your instance\.

\-or\-

If you are targeting an EC2 instance for Windows Server instead of Linux, replace df in the following command with ipconfig\. Output from this command lists details about the IP address, subnet mask, and default gateway for adapters on the target instance\.

When you are ready to register other task types, or use more of the available Run Command options, see [Examples: Register tasks with a maintenance window](mw-cli-register-tasks-examples.md)\. There, we provide more information about all four task types, and some of their most important options, to help you plan for more extensive real\-world scenarios\. 

**To register a task with a maintenance window**

1. Run the following command on your local machine\. The version to run from a local Windows machine includes the escape characters \("/"\) that you need to run the command from your command line tool\.

------
#### [ Linux ]

   ```
   aws ssm register-task-with-maintenance-window \
       --window-id mw-0c50858d01EXAMPLE \
       --task-arn "AWS-RunShellScript" \
       --max-concurrency 1 --max-errors 1 \
       --priority 10 \
       --targets "Key=InstanceIds,Values=i-0471e04240EXAMPLE" \
       --task-type "RUN_COMMAND" \
       --task-invocation-parameters '{"RunCommand":{"Parameters":{"commands":["df"]}}}'
   ```

------
#### [ Windows ]

   ```
   aws ssm register-task-with-maintenance-window ^
       --window-id mw-0c50858d01EXAMPLE ^
       --task-arn "AWS-RunShellScript" ^
       --max-concurrency 1 --max-errors 1 ^
       --priority 10 ^
       --targets "Key=InstanceIds,Values=i-02573cafcfEXAMPLE" ^
       --task-type "RUN_COMMAND" ^
       --task-invocation-parameters={\"RunCommand\":{\"Parameters\":{\"commands\":[\"df\"]}}}
   ```

------

   The system returns information similar to the following:

   ```
   {
       "WindowTaskId": "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE"
   }
   ```

1. Now run the following command to view details about the maintenance window task you created\. 

------
#### [ Linux ]

   ```
   aws ssm describe-maintenance-window-tasks \
       --window-id mw-0c50858d01EXAMPLE
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-maintenance-window-tasks ^
       --window-id mw-0c50858d01EXAMPLE
   ```

------

1. The system returns information similar to the following:

   ```
   {
       "Tasks": [
           {
               "WindowId": "mw-0c50858d01EXAMPLE",
               "WindowTaskId": "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE",
               "TaskArn": "AWS-RunShellScript",
               "Type": "RUN_COMMAND",
               "Targets": [
                   {
                       "Key": "InstanceIds",
                       "Values": [
                           "i-02573cafcfEXAMPLE"
                       ]
                   }
               ],
               "TaskParameters": {},
               "Priority": 10,
               "ServiceRoleArn": "arn:aws:iam::123456789012:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM",
               "MaxConcurrency": "1",
               "MaxErrors": "1"
           }
       ]
   }
   ```

1. Wait until the task has had time to run, based on the schedule you specified in [Step 1: Create the maintenance window \(AWS CLI\)](mw-cli-tutorial-create-mw.md)\. For example, if you specified **\-\-schedule "rate\(5 minutes\)"**, wait five minutes\. Then run the following command to view information about any executions that occurred for this task\. 

------
#### [ Linux ]

   ```
   aws ssm describe-maintenance-window-executions \
       --window-id mw-0c50858d01EXAMPLE
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-maintenance-window-executions ^
       --window-id mw-0c50858d01EXAMPLE
   ```

------

   The system returns information similar to the following:

   ```
   {
       "WindowExecutions": [
           {
               "WindowId": "mw-0c50858d01EXAMPLE",
               "WindowExecutionId": "14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE",
               "Status": "SUCCESS",
               "StartTime": 1557593493.096,
               "EndTime": 1557593498.611
           }
       ]
   }
   ```

**Tip**  
After the task completes successfully, you can decrease the rate at which the maintenance window runs\. For example, run the following command to decrease the frequency to once a week:  

```
aws ssm update-maintenance-window \
    --window-id mw-0c50858d01EXAMPLE \
    --schedule "rate(7 days)"
```

```
aws ssm update-maintenance-window ^
    --window-id mw-0c50858d01EXAMPLE ^
    --schedule "rate(7 days)"
```
For information about managing maintenance window schedules, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md) and [Reference: Maintenance window scheduling and active period options](maintenance-windows-schedule-options.md)\.  
For information about using the AWS CLI to modify a maintenance window, see [Tutorial: Update a maintenance window \(AWS CLI\)](maintenance-windows-cli-tutorials-update.md)\.

For practice running AWS CLI commands to view more details about your maintenance window task and its executions, continue to [Tutorial: View information about tasks and task executions \(AWS CLI\)](mw-cli-tutorial-task-info.md)\.

**About tutorial command output**  
It's beyond the scope of this tutorial to use the AWS CLI to view the *output* of the Run Command command associated with your maintenance window task executions\.

You could view this data, however, using the AWS CLI\. \(You could also view the output in the Systems Manager console or in a log file stored in an S3 bucket, if you had configured the maintenance window to store command output there\.\) You would find that the output of the df command on an EC2 instance for Linux is similar to the following:

```
Filesystem 1K-blocks Used Available Use% Mounted on

devtmpfs 485716 0 485716 0% /dev

tmpfs 503624 0 503624 0% /dev/shm

tmpfs 503624 328 503296 1% /run

tmpfs 503624 0 503624 0% /sys/fs/cgroup

/dev/xvda1 8376300 1464160 6912140 18% /
```

The output of the ipconfig command on an EC2 instance for Windows Server is similar to the following:

```
Windows IP Configuration


Ethernet adapter Ethernet 2:

   Connection-specific DNS Suffix  . : example.com
   IPv4 Address. . . . . . . . . . . : 10.24.34.0/23
   Subnet Mask . . . . . . . . . . . : 255.255.255.255
   Default Gateway . . . . . . . . . : 0.0.0.0

Ethernet adapter Ethernet:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . : abc1.wa.example.net

Wireless LAN adapter Local Area Connection* 1:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :

Wireless LAN adapter Wi-Fi:

   Connection-specific DNS Suffix  . :
   Link-local IPv6 Address . . . . . : fe80::100b:c234:66d6:d24f%4
   IPv4 Address. . . . . . . . . . . : 192.0.2.0
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 192.0.2.0

Ethernet adapter Bluetooth Network Connection:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :
```