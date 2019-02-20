# Tutorial: View Information About Tasks and Task Executions \(AWS CLI\)<a name="mw-cli-tutorial-task-info"></a>

This tutorial demonstrates how to use the AWS CLI to view details about your completed Maintenance Window executions\. 

As you follow the steps in this tutorial, replace the values in *red* with your own options and IDs\. For example, replace the Maintenance Window ID *mw\-0c5ed765acEXAMPLE* and the instance ID *i\-1234567890EXAMPLE* with IDs from resources you have created\.

If you are continuing directly from [Tutorial: Create and Configure a Maintenance Window \(AWS CLI\)](maintenance-windows-cli-tutorials-create.md), make sure you have allowed enough time for your Maintenance Window to run at least once in order to see its execution results\.

**To view information about tasks and task executions \(AWS CLI\)**

1. Run the following command to view a list of task executions for a specific Maintenance Window:

   ```
   aws ssm describe-maintenance-window-executions --window-id "mw-0c5ed765acEXAMPLE"
   ```

   The system returns information like the following:

   ```
   {
      "WindowExecutions":[
         {
            "Status":"SUCCESS",
            "WindowExecutionId":"1111-1111-1111-11111",
            "StartTime":1478230495.469
         },
         {
            "Status":"SUCCESS",
            "WindowExecutionId":"2222-2-2-22222222-22",
            "StartTime":1478231395.677
         },
         {
            "Status":"SUCCESS",
            "WindowExecutionId":"33333-333-333-3333333",
            "StartTime":1478272795.021
         },
         {
            "Status":"SUCCESS",
            "WindowExecutionId":"4444-44-44-44444444",
            "StartTime":1478273694.932
         }
      ],
      "NextToken":90gfhS4JD8aEXAMPLE..."
   }
   ```

1. Run the following command to get information about a Maintenance Window task execution:

   ```
   aws ssm get-maintenance-window-execution --window-execution-id "1a2b3c4d-1a2b-1a2b-1a2b-EXAMPLEa-1a2"
   ```

   The system returns information like the following:

   ```
   {
      "Status":"SUCCESS",
      "TaskIds":[
         "333-33-3333-333333"
      ],
      "StartTime":1478230495.472,
      "EndTime":1478230516.505,
      "WindowExecutionId":"1a2b3c4d-1a2b-1a2b-1a2b-EXAMPLEa-1a2"
   }
   ```

1. Run the following command to list the tasks run as part of a Maintenance Window execution:

   ```
   aws ssm describe-maintenance-window-execution-tasks --window-execution-id "1a2b3c4d-1a2b-1a2b-1a2b-EXAMPLEa-1a2"
   ```

   The system returns information like the following:

   ```
   {
      "WindowExecutionTaskIdentities":[
         {
            "Status":"SUCCESS",
            "EndTime":1478230516.425,
            "StartTime":1478230495.782,
            "TaskId":"33333-333-333-3333333"
         }
      ]
   }
   ```

1. Run the following command to get the details of a task execution:

   ```
   aws ssm get-maintenance-window-execution-task --window-execution-id "1a2b3c4d-1a2b-1a2b-1a2b-EXAMPLEa-1a2" --task-id "6420cb42-5dbd-4060-8349-bfb2fEXAMPLE"
   ```

   The system returns information like the following:

   ```
   {
      "Status":"SUCCESS",
      "MaxErrors":"1",
      "TaskArn":"AWS-RunPowerShellScript",
      "MaxConcurrency":"1",
      "ServiceRole":"arn:aws:iam::333333333:role/MW-Role",
      "WindowExecutionId":"555555-555-55-555555",
      "Priority":0,
      "StartTime":1478230495.782,
      "EndTime":1478230516.425,
      "Type":"RUN_COMMAND",
      "TaskParameters":[
   
      ],
      "TaskExecutionId":"4444-4444-4444-444444"
   }
   ```

1. Run the following command to get the specific task invocations performed for a task execution\.

   ```
   aws ssm describe-maintenance-window-execution-task-invocations --window-execution-id "555555-555-55-555555" --task-id "6420cb42-5dbd-4060-8349-bfb2fEXAMPLE"
   ```

   The system returns information like the following\.

   ```
   {
      "WindowExecutionTaskInvocationIdentities":[
         {
            "Status":"SUCCESS",
            "Parameters":"{\" documentName \" : \" AWS-RunPowerShellScript \" , \" instanceIds \" :[ \" i-1a2b3c4d5e6f7g8h9 \" , \" i-0a
   00def7faa94f1dc \" ], \" parameters \" :{ \" commands \" :[ \" ipconfig.exe \" ]}, \" maxConcurrency \" : \" 1 \" , \" maxErrors \" : \" 1 \" }",
            "ExecutionId":"555555-555-55-555555",
            "InvocationId":"3333-33333-3333-33333",
            "StartTime":1478230495.842,
            "EndTime":1478230516.291
         }
      ]
   }
   ```