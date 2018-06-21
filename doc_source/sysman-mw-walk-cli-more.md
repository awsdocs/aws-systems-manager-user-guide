# Walkthrough: List Information About Maintenance Windows<a name="sysman-mw-walk-cli-more"></a>

This section includes commands to help you update or get information about your Maintenance Windows, tasks, executions, and invocations\.

**List All Maintenance Windows in Your AWS Account**  
Run the command as shown here\.

```
aws ssm describe-maintenance-windows
```

The system returns information like the following\.

```
{
   "WindowIdentities":[
      {
         "Duration":2,
         "Cutoff":0,
         "WindowId":"mw-ab12cd34ef56gh78",
         "Enabled":true,
         "Name":"IAD-Every-15-Minutes"
      },
      {
         "Duration":4,
         "Cutoff":1,
         "WindowId":"mw-1a2b3c4d5e6f7g8h9",
         "Enabled":true,
         "Name":"My-First-Maintenance-Window"
      },
      {
         "Duration":8,
         "Cutoff":2,
         "WindowId":"mw-123abc456def789",
         "Enabled":false,
         "Name":"Every-Day"
      }
   ]
}
```

**List all enabled Maintenance Windows**  
Run the command as shown here\.

```
aws ssm describe-maintenance-windows --filters "Key=Enabled,Values=true"
```

The system returns information like the following\.

```
{
   "WindowIdentities":[
      {
         "Duration":2,
         "Cutoff":0,
         "WindowId":"mw-ab12cd34ef56gh78",
         "Enabled":true,
         "Name":"IAD-Every-15-Minutes"
      },
      {
         "Duration":4,
         "Cutoff":1,
         "WindowId":"mw-1a2b3c4d5e6f7g8h9",
         "Enabled":true,
         "Name":"My-First-Maintenance-Window"
      }
   ]
}
```

**List all Disabled Maintenance Windows**  
Run the command as shown here\.

```
aws ssm describe-maintenance-windows --filters "Key=Enabled,Values=false"
```

The system returns information like the following\.

```
{
   "WindowIdentities":[
      {
         "Duration":8,
         "Cutoff":2,
         "WindowId":"mw-1a2b3c4d5e6f7g8h9",
         "Enabled":false,
         "Name":"Every-Day"
      }
   ]
}
```

**Filter by Name**  
In this example, the command returns all Maintenance Windows with a name starting with 'My'\.

```
aws ssm describe-maintenance-windows --filters "Key=Name,Values=My"
```

The system returns information like the following\.

```
{
   "WindowIdentities":[
      {
         "Duration":4,
         "Cutoff":1,
         "WindowId":"mw-1a2b3c4d5e6f7g8h9",
         "Enabled":true,
         "Name":"My-First-Maintenance-Window"
      }
   ]
}
```

**Display the Targets for a Maintenance Window Matching a Specific Owner Information Value**  
Run the command as shown here\.

```
aws ssm describe-maintenance-window-targets --window-id "mw-ab12cd34ef56gh78" --filters "Key=OwnerInformation,Values=Single instance"
```

The system returns information like the following\.

```
{
   "Targets":[
      {
         "TargetType":"INSTANCE",
         "TagFilters":[

         ],
         "TargetIds":[
            "i-1a2b3c4d5e6f7g8h9"
         ],
         "WindowTargetId":"1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d-1a2",
         "OwnerInformation":"Single instance"
      }
   ]
}
```

**Show All Registered Tasks that Invoke the AWS\-RunPowerShellScript Run Command**  
Run the command as shown here\.

```
aws ssm describe-maintenance-window-tasks --window-id "mw-ab12cd34ef56gh78" --filters "Key=TaskArn,Values=AWS-RunPowerShellScript"
```

The system returns information like the following\.

```
{
   "Tasks":[
      {
         "ServiceRoleArn":"arn:aws:iam::444444444444:role/MW-Role",
         "MaxErrors":"1",
         "TaskArn":"AWS-RunPowerShellScript",
         "MaxConcurrency":"1",
         "WindowTaskId":"1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d5e6c",
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
               "TaskTargetId":"i-1a2b3c4d5e6f7g8h9",
               "TaskTargetType":"INSTANCE"
            }
         ]
      },
      {
         "ServiceRoleArn":"arn:aws:iam::333333333333:role/MW-Role",
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

**Show All Registered Tasks that Have a Priority of 3**  
Run the command as shown here\.

```
aws ssm describe-maintenance-window-tasks --window-id "mw-ab12cd34ef56gh78" --filters "Key=Priority,Values=3"
```

The system returns information like the following\.

```
{
   "Tasks":[
      {
         "ServiceRoleArn":"arn:aws:iam::222222222:role/MW-Role",
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
               "TaskTargetId":"i-1a2b3c4d5e6f7g8h9",
               "TaskTargetType":"INSTANCE"
            }
         ]
      }
   ]
}
```

**Show All Registered Tasks that Have a Priority of 1 and Use Run Command**  
Run the command as shown here\.

```
aws ssm describe-maintenance-window-tasks --window-id "mw-ab12cd34ef56gh78" --filters "Key=Priority,Values=1" "Key=TaskType,Values=RUN_COMMAND"
```

The system returns information like the following\.

```
{
   "Tasks":[
      {
         "ServiceRoleArn":"arn:aws:iam::333333333:role/MW-Role",
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

**List All Tasks Run Before a Date**  
Run the command as shown here\.

```
aws ssm describe-maintenance-window-executions --window-id "mw-ab12cd34ef56gh78" --filters "Key=ExecutedBefore,Values=2016-11-04T05:00:00Z"
```

The system returns information like the following\.

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
         "WindowExecutionId":"06dc5f8a-9ef0-4ae9-a466-ada2d4ce2d22",
         "StartTime":1478230495.469
      },
      {
         "Status":"SUCCESS",
         "WindowExecutionId":"57ad6419-023e-44b0-a831-6687334390b2",
         "StartTime":1478231395.677
      },
      {
         "Status":"SUCCESS",
         "WindowExecutionId":"ed1372b7-866b-4d64-bc2a-bbfd5195f4ae",
         "StartTime":1478232295.529
      },
      {
         "Status":"SUCCESS",
         "WindowExecutionId":"154eb2fa-6390-4cb7-8c9e-55686b88c7b3",
         "StartTime":1478233195.687
      },
      {
         "Status":"SUCCESS",
         "WindowExecutionId":"1c4de752-eff6-4778-b477-1681c6c03cf1",
         "StartTime":1478234095.553
      },
      {
         "Status":"SUCCESS",
         "WindowExecutionId":"56062f75-e4d8-483f-b5c2-906d613409a4",
         "StartTime":1478234995.12
      }
   ]
}
```

**List All Tasks Run After a Date**  
Run the command as shown here\.

```
aws ssm describe-maintenance-window-executions --window-id "mw-ab12cd34ef56gh78" --filters "Key=ExecutedAfter,Values=2016-11-04T17:00:00Z"
```

The system returns information like the following\.

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