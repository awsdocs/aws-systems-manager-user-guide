# Tutorial: View Information About Maintenance Windows \(AWS CLI\)<a name="maintenance-windows-cli-tutorials-describe"></a>

This tutorial includes commands to help you update or get information about your Maintenance Windows, tasks, executions, and invocations\. The examples are organized by command to demonstrate how to use command options to filter for the type of detail you want to see\.

As you follow the steps in this tutorial, replace the values in italicized *red* text with your own options and IDs\. For example, replace the Maintenance Window ID *mw\-0c5ed765acEXAMPLE* and the instance ID *i\-1234567890EXAMPLE* with IDs from resources you create\.

For information about setting up and configuring the CLI, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) and [Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html)\.

**Topics**
+ [Examples for 'describe\-maintenance\-windows'](#mw-cli-tutorials-describe-maintenance-windows)
+ [Examples for 'describe\-maintenance\-window\-targets'](#mw-cli-tutorials-describe-maintenance-window-targets)
+ [Examples for 'describe\-maintenance\-window\-tasks'](#mw-cli-tutorials-describe-maintenance-window-tasks)
+ [Examples for 'describe\-maintenance\-windows\-for\-target'](#mw-cli-tutorials-describe-maintenance-windows-for-target)
+ [Examples for 'describe\-maintenance\-window\-executions'](#mw-cli-tutorials-describe-maintenance-window-executions)
+ [Examples for 'describe\-maintenance\-window\-schedule'](#mw-cli-tutorials-describe-maintenance-window-schedule)

## Examples for 'describe\-maintenance\-windows'<a name="mw-cli-tutorials-describe-maintenance-windows"></a>

**List all Maintenance Windows in your AWS account**  
Run the following command:

```
aws ssm describe-maintenance-windows
```

The system returns information like the following:

```
{
   "WindowIdentities":[
      {
         "Duration":2,
         "Cutoff":0,
         "WindowId":"mw-0c5ed765acEXAMPLE",
         "Enabled":true,
         "Name":"IAD-Every-15-Minutes"
      },
      {
         "Duration":4,
         "Cutoff":1,
         "WindowId":"mw-9a8b7c6d5eEXAMPLE",
         "Enabled":true,
         "Name":"My-First-Maintenance-Window"
      },
      {
         "Duration":8,
         "Cutoff":2,
         "WindowId":"mw-369258147YEXAMPLE",
         "Enabled":false,
         "Name":"Every-Day"
      }
   ]
}
```

**List all enabled Maintenance Windows**  
Run the following command:

```
aws ssm describe-maintenance-windows --filters "Key=Enabled,Values=true"
```

The system returns information like the following:

```
{
   "WindowIdentities":[
      {
         "Duration":2,
         "Cutoff":0,
         "WindowId":"mw-0c5ed765acEXAMPLE",
         "Enabled":true,
         "Name":"IAD-Every-15-Minutes"
      },
      {
         "Duration":4,
         "Cutoff":1,
         "WindowId":"mw-9a8b7c6d5eEXAMPLE",
         "Enabled":true,
         "Name":"My-First-Maintenance-Window"
      }
   ]
}
```

**List all disabled Maintenance Windows**  
Run the following command:

```
aws ssm describe-maintenance-windows --filters "Key=Enabled,Values=false"
```

The system returns information like the following:

```
{
   "WindowIdentities":[
      {
         "Duration":8,
         "Cutoff":2,
         "WindowId":"mw-369258147YEXAMPLE",
         "Enabled":false,
         "Name":"Every-Day"
      }
   ]
}
```

**List all Maintenance Windows having names that start with a certain prefix**  
Run the following command:

```
aws ssm describe-maintenance-windows --filters "Key=Name,Values=My"
```

The system returns information like the following:

```
{
   "WindowIdentities":[
      {
         "Duration":4,
         "Cutoff":1,
         "WindowId":"mw-369258147YEXAMPLE",
         "Enabled":true,
         "Name":"My-First-Maintenance-Window"
      }
   ]
}
```

## Examples for 'describe\-maintenance\-window\-targets'<a name="mw-cli-tutorials-describe-maintenance-window-targets"></a>

**Display the targets for a Maintenance Window matching a specific owner information value**  
Run the following command:

```
aws ssm describe-maintenance-window-targets --window-id "mw-ab12cd34eEXAMPLE" --filters "Key=OwnerInformation,Values=Single instance"
```

The system returns information like the following:

```
{
   "Targets":[
      {
         "TargetType":"INSTANCE",
         "TagFilters":[

         ],
         "TargetIds":[
            "i-1234567890EXAMPLE"
         ],
         "WindowTargetId":"1a2b3c4d-1a2b-1a2b-1a2b-EXAMPLE1-1a2",
         "OwnerInformation":"Single instance"
      }
   ]
}
```

## Examples for 'describe\-maintenance\-window\-tasks'<a name="mw-cli-tutorials-describe-maintenance-window-tasks"></a>

**Show all registered tasks that invoke the AWS\-RunPowerShellScript Run Command**  
Run the following command:

```
aws ssm describe-maintenance-window-tasks --window-id "mw-9a8b7c6d5eEXAMPLE" --filters "Key=TaskArn,Values=AWS-RunPowerShellScript"
```

The system returns information like the following:

```
{
   "Tasks":[
      {
         "ServiceRoleArn":"arn:aws:iam::111122223333:role/MW-Role",
         "MaxErrors":"1",
         "TaskArn":"AWS-RunPowerShellScript",
         "MaxConcurrency":"1",
         "WindowTaskId":"1a2b3c4d-1a2b-1a2b-1a2b-1a2b3EXAMPLE",
         "TaskParameters":{
            "commands":{
               "Values":[
                  "driverquery.exe"
               ]
            }
         },
         "Priority":3,
         "Type":"RUN_COMMAND",
         "Targets":[
            {
               "TaskTargetId":"i-1234567890EXAMPLE",
               "TaskTargetType":"INSTANCE"
            }
         ]
      },
      {
         "ServiceRoleArn":"arn:aws:iam::111122223333:role/MW-Role",
         "MaxErrors":"1",
         "TaskArn":"AWS-RunPowerShellScript",
         "MaxConcurrency":"1",
         "WindowTaskId":"33333-33333-333-33333",
         "TaskParameters":{
            "commands":{
               "Values":[
                  "ipconfig.exe"
               ]
            }
         },
         "Priority":1,
         "Type":"RUN_COMMAND",
         "Targets":[
            {
               "TaskTargetId":"44444-444-4444-444444",
               "TaskTargetType":"WINDOW_TARGET"
            }
         ]
      }
   ]
}
```

**Show all registered tasks that have a priority of "3"**  
Run the following command:

```
aws ssm describe-maintenance-window-tasks --window-id "mw-9a8b7c6d5eEXAMPLE" --filters "Key=Priority,Values=3"
```

The system returns information like the following:

```
{
   "Tasks":[
      {
         "ServiceRoleArn":"arn:aws:iam::111122223333:role/MW-Role",
         "MaxErrors":"1",
         "TaskArn":"AWS-RunPowerShellScript",
         "MaxConcurrency":"1",
         "WindowTaskId":"333333-333-33333-33333",
         "TaskParameters":{
            "commands":{
               "Values":[
                  "driverquery.exe"
               ]
            }
         },
         "Priority":3,
         "Type":"RUN_COMMAND",
         "Targets":[
            {
               "TaskTargetId":"i-1234567890EXAMPLE",
               "TaskTargetType":"INSTANCE"
            }
         ]
      }
   ]
}
```

**Show all registered tasks that have a priority of "1" and use Run Command**  
Run the following command:

```
aws ssm describe-maintenance-window-tasks --window-id "mw-ab12cd34ef56gh78" --filters "Key=Priority,Values=1" "Key=TaskType,Values=RUN_COMMAND"
```

The system returns information like the following:

```
{
   "Tasks":[
      {
         "ServiceRoleArn":"arn:aws:iam::111122223333:role/MW-Role",
         "MaxErrors":"1",
         "TaskArn":"AWS-RunPowerShellScript",
         "MaxConcurrency":"1",
         "WindowTaskId":"66666-555-66-555-6666",
         "TaskParameters":{
            "commands":{
               "Values":[
                  "ipconfig.exe"
               ]
            }
         },
         "Priority":1,
         "Type":"RUN_COMMAND",
         "Targets":[
            {
               "TaskTargetId":"777-77-777-7777777",
               "TaskTargetType":"WINDOW_TARGET"
            }
         ]
      }
   ]
}
```

## Examples for 'describe\-maintenance\-windows\-for\-target'<a name="mw-cli-tutorials-describe-maintenance-windows-for-target"></a>

**List information about the Maintenance Window targets or tasks associated with instances tagged with a particular key**  
Run the following command:

```
aws ssm describe-maintenance-windows-for-target --resource-type INSTANCE --targets "Key=tag-key,Values=prod"
```

The system returns information like the following:

```
{
    "WindowIdentities": [
        {
            "WindowId": "mw-9a8b7c6d5eEXAMPLE",
            "Name": "DemoRateStartDate"
        }
    ]
}
```

**List information about the Maintenance Window targets or tasks associated with instances tagged with a particular key\-value pair**  
Run the following command:

```
aws ssm describe-maintenance-windows-for-target --resource-type INSTANCE --targets "Key=tag:prod,Values=rhel7"
```

The system returns information like the following:

```
{
    "WindowIdentities": [
        {
            "WindowId": "mw-9a8b7c6d5eEXAMPLE",
            "Name": "DemoCronEndDate"
        }
    ]
}
```

**List information about the Maintenance Window targets or tasks associated with a specific instance**  
Run the following command:

```
aws ssm describe-maintenance-windows-for-target --resource-type INSTANCE --targets "Key=InstanceIds,Values=i-1234567890EXAMPLE" --max-results 10
```

The system returns information like the following:

```
{
    "WindowIdentities": [
        {
            "WindowId": "mw-0c5ed765acEXAMPLE",
            "Name": "DemoRateStartDate"
        },
        {
            "WindowId": "mw-9a8b7c6d5eEXAMPLE",
            "Name": "DemoCronEndDate"
        }
    ]
}
```

## Examples for 'describe\-maintenance\-window\-executions'<a name="mw-cli-tutorials-describe-maintenance-window-executions"></a>

**List all tasks run before a certain date**  
Run the following command:

```
aws ssm describe-maintenance-window-executions --window-id "111122223333" --filters "Key=ExecutedBefore,Values=2016-11-04T05:00:00Z"
```

The system returns information like the following:

```
{
   "WindowExecutions":[
      {
         "Status":"SUCCESS",
         "EndTime":1478229594.666,
         "WindowExecutionId":"",
         "StartTime":1478229594.666
      },
      {
         "Status":"SUCCESS",
         "WindowExecutionId":"06dc5f8a-9ef0-4ae9-a466-ada2dEXAMPLE",
         "StartTime":1478230495.469
      },
      {
         "Status":"SUCCESS",
         "WindowExecutionId":"57ad6419-023e-44b0-a831-66873EXAMPLE",
         "StartTime":1478231395.677
      },
      {
         "Status":"SUCCESS",
         "WindowExecutionId":"ed1372b7-866b-4d64-bc2a-bbfd5EXAMPLE",
         "StartTime":1478232295.529
      },
      {
         "Status":"SUCCESS",
         "WindowExecutionId":"154eb2fa-6390-4cb7-8c9e-55686EXAMPLE",
         "StartTime":1478233195.687
      },
      {
         "Status":"SUCCESS",
         "WindowExecutionId":"1c4de752-eff6-4778-b477-1681cEXAMPLE",
         "StartTime":1478234095.553
      },
      {
         "Status":"SUCCESS",
         "WindowExecutionId":"56062f75-e4d8-483f-b5c2-906d6EXAMPLE",
         "StartTime":1478234995.12
      }
   ]
}
```

**List all tasks run after a certain date**  
Run the following command:

```
aws ssm describe-maintenance-window-executions --window-id "mw-9a8b7c6d5eEXAMPLE" --filters "Key=ExecutedAfter,Values=2016-11-04T17:00:00Z"
```

The system returns information like the following:

```
{
   "WindowExecutions":[
      {
         "Status":"SUCCESS",
         "WindowExecutionId":"33333-4444-444-5555555",
         "StartTime":1478279095.042
      },
      {
         "Status":"SUCCESS",
         "WindowExecutionId":"55555-6666-6666-777777",
         "StartTime":1478279994.958
      },
      {
         "Status":"SUCCESS",
         "WindowExecutionId":"8888-888-888-888888",
         "StartTime":1478280895.149
      }
   ]
}
```

## Examples for 'describe\-maintenance\-window\-schedule'<a name="mw-cli-tutorials-describe-maintenance-window-schedule"></a>

**Display the next ten scheduled Maintenance Window runs for a particular instance**  
Run the following command:

```
aws ssm describe-maintenance-window-schedule --resource-type INSTANCE --targets "Key=InstanceIds,Values=i-456jkl321EXAMPLE" --max-results 10
```

The system returns information like the following:

```
{
    "ScheduledWindowExecutions": [
        {
            "WindowId": "mw-0c5ed765acEXAMPLE",
            "Name": "DemoRateStartDate",
            "ExecutionTime": "2018-10-20T05:34:56-07:00"
        },
        {
            "WindowId": "mw-0c5ed765acEXAMPLE",
            "Name": "DemoRateStartDate",
            "ExecutionTime": "2018-10-21T05:34:56-07:00"
        },
        {
            "WindowId": "mw-0c5ed765acEXAMPLE",
            "Name": "DemoRateStartDate",
            "ExecutionTime": "2018-10-22T05:34:56-07:00"
        },
        {
            "WindowId": "mw-0c5ed765acEXAMPLE",
            "Name": "DemoRateStartDate",
            "ExecutionTime": "2018-10-23T05:34:56-07:00"
        },
        {
            "WindowId": "mw-0bc0739f959f09230",
            "Name": "DemoCronEndDate",
            "ExecutionTime": "2018-10-23T16:00Z"
        },
        {
            "WindowId": "mw-0c5ed765acEXAMPLE",
            "Name": "DemoRateStartDate",
            "ExecutionTime": "2018-10-24T05:34:56-07:00"
        },
        {
            "WindowId": "mw-9a8b7c6d5eEXAMPLE",
            "Name": "DemoCronEndDate",
            "ExecutionTime": "2018-10-24T16:00Z"
        },
        {
            "WindowId": "mw-0c5ed765acEXAMPLE",
            "Name": "DemoRateStartDate",
            "ExecutionTime": "2018-10-25T05:34:56-07:00"
        },
        {
            "WindowId": "mw-9a8b7c6d5eEXAMPLE",
            "Name": "DemoCronEndDate",
            "ExecutionTime": "2018-10-25T16:00Z"
        },
        {
            "WindowId": "mw-0c5ed765acEXAMPLE",
            "Name": "DemoRateStartDate",
            "ExecutionTime": "2018-10-26T05:34:56-07:00"
        }
    ],
    "NextToken": "AAEABUXdceT92FvtKld/dGHELj5Mi+GKW/EXAMPLE"
}
```

**Display the Maintenance Window schedule for instances tagged with a certain key\-value pair**  
Run the following command:

```
aws ssm describe-maintenance-window-schedule --resource-type INSTANCE --targets "Key=tag:prod,Values=rhel7"
```

The system returns information like the following:

```
{
    "ScheduledWindowExecutions": [
        {
            "WindowId": "mw-0c5ed765acEXAMPLE",
            "Name": "DemoRateStartDate",
            "ExecutionTime": "2018-10-20T05:34:56-07:00"
        },
        {
            "WindowId": "mw-0c5ed765acEXAMPLE",
            "Name": "DemoRateStartDate",
            "ExecutionTime": "2018-10-21T05:34:56-07:00"
        },
        {
            "WindowId": "mw-0c5ed765acEXAMPLE",
            "Name": "DemoRateStartDate",
            "ExecutionTime": "2018-10-22T05:34:56-07:00"
        },
        {
            "WindowId": "mw-0c5ed765acEXAMPLE",
            "Name": "DemoRateStartDate",
            "ExecutionTime": "2018-10-23T05:34:56-07:00"
        },
        {
            "WindowId": "mw-0c5ed765acEXAMPLE",
            "Name": "DemoRateStartDate",
            "ExecutionTime": "2018-10-24T05:34:56-07:00"
        }
    ],
    "NextToken": "AAEABccwSXqQRGKiTZ1yzGELR6cxW4W/EXAMPLE"
}
```

**Display start times for next four runs of a Maintenance Window**  
Run the following command:

```
aws ssm describe-maintenance-window-schedule --window-id "mw-ab12cd34eEXAMPLE" --max-results "4"
```

The system returns information like the following:

```
{
    "WindowSchedule": [
        {
            "ScheduledWindowExecutions": [
                {
                    "ExecutionTime": "2018-10-04T10:10:10Z",
                    "Name": "My-Maintenance-Window",
                    "WindowId": "mw-0c5ed765acEXAMPLE"
                },
                {
                    "ExecutionTime": "2018-10-11T10:10:10Z",
                    "Name": "My-Maintenance-Window",
                    "WindowId": "mw-0c5ed765acEXAMPLE"
                },
                {
                    "ExecutionTime": "2018-10-18T10:10:10Z",
                    "Name": "My-Maintenance-Window",
                    "WindowId": "mw-0c5ed765acEXAMPLE"
                },
                {
                    "ExecutionTime": "2018-10-25T10:10:10Z",
                    "Name": "My-Maintenance-Window",
                    "WindowId": "mw-0c5ed765acEXAMPLE"
                }
            ]
        }
    ]
}
```