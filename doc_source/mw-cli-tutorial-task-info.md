# Tutorial: View information about tasks and task executions \(AWS CLI\)<a name="mw-cli-tutorial-task-info"></a>

This tutorial demonstrates how to use the AWS CLI to view details about your completed maintenance window task executions\. 

If you are continuing directly from [Tutorial: Create and configure a maintenance window \(AWS CLI\)](maintenance-windows-cli-tutorials-create.md), make sure you have allowed enough time for your maintenance window to run at least once in order to see its execution results\.

As you follow the steps in this tutorial, replace the values in italicized *red* text with your own options and IDs\. For example, replace the maintenance window ID *mw\-0c50858d01EXAMPLE* and the instance ID *i\-02573cafcfEXAMPLE* with IDs of resources you create\.

**To view information about tasks and task executions \(AWS CLI\)**

1. Run the following command to view a list of task executions for a specific maintenance window\.

------
#### [ Linux ]

   ```
   aws ssm describe-maintenance-window-executions \
       --window-id "mw-0c50858d01EXAMPLE"
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-maintenance-window-executions ^
       --window-id "mw-0c50858d01EXAMPLE"
   ```

------

   The system returns information like the following\.

   ```
   {
       "WindowExecutions": [
           {
               "WindowId": "mw-0c50858d01EXAMPLE",
               "WindowExecutionId": "14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE",
               "Status": "SUCCESS",
               "StartTime": 1557593793.483,
               "EndTime": 1557593798.978
           },
           {
               "WindowId": "mw-0c50858d01EXAMPLE",
               "WindowExecutionId": "791b72e0-f0da-4021-8b35-f95dfEXAMPLE",
               "Status": "SUCCESS",
               "StartTime": 1557593493.096,
               "EndTime": 1557593498.611
           },
           {
               "WindowId": "mw-0c50858d01EXAMPLE",
               "WindowExecutionId": "ecec60fa-6bb0-4d26-98c7-140308EXAMPLE",
               "Status": "SUCCESS",
               "StatusDetails": "No tasks to execute.",
               "StartTime": 1557593193.309,
               "EndTime": 1557593193.334
           }
       ]
   }
   ```

1. Run the following command to get information about a maintenance window task execution\.

------
#### [ Linux ]

   ```
   aws ssm get-maintenance-window-execution \
       --window-execution-id "14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE"
   ```

------
#### [ Windows ]

   ```
   aws ssm get-maintenance-window-execution ^
       --window-execution-id "14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE"
   ```

------

   The system returns information like the following\.

   ```
   {
       "WindowExecutionId": "14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE",
       "TaskIds": [
           "c9b05aba-197f-4d8d-be34-e73fbEXAMPLE"
       ],
       "Status": "SUCCESS",
       "StartTime": 1557593493.096,
       "EndTime": 1557593498.611
   }
   ```

1. Run the following command to list the tasks run as part of a maintenance window execution\.

------
#### [ Linux ]

   ```
   aws ssm describe-maintenance-window-execution-tasks \
       --window-execution-id "14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE"
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-maintenance-window-execution-tasks ^
       --window-execution-id "14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE"
   ```

------

   The system returns information like the following\.

   ```
   {
       "WindowExecutionTaskIdentities": [
           {
               "WindowExecutionId": "14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE",
               "TaskExecutionId": "c9b05aba-197f-4d8d-be34-e73fbEXAMPLE",
               "Status": "SUCCESS",
               "StartTime": 1557593493.162,
               "EndTime": 1557593498.57,
               "TaskArn": "AWS-RunShellScript",
               "TaskType": "RUN_COMMAND"
           }
       ]
   }
   ```

1. Run the following command to get the details of a task execution\.

------
#### [ Linux ]

   ```
   aws ssm get-maintenance-window-execution-task \
       --window-execution-id "14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE" \
       --task-id "c9b05aba-197f-4d8d-be34-e73fbEXAMPLE"
   ```

------
#### [ Windows ]

   ```
   aws ssm get-maintenance-window-execution-task ^
       --window-execution-id "14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE" ^
       --task-id "c9b05aba-197f-4d8d-be34-e73fbEXAMPLE"
   ```

------

   The system returns information like the following\.

   ```
   {
       "WindowExecutionId": "14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE",
       "TaskExecutionId": "c9b05aba-197f-4d8d-be34-e73fbEXAMPLE",
       "TaskArn": "AWS-RunShellScript",
       "ServiceRole": "arn:aws:iam::111122223333:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM",
       "Type": "RUN_COMMAND",
       "TaskParameters": [
           {
               "aws:InstanceId": {
                   "Values": [
                       "i-02573cafcfEXAMPLE"
                   ]
               },
               "commands": {
                   "Values": [
                       "df"
                   ]
               }
           }
       ],
       "Priority": 10,
       "MaxConcurrency": "1",
       "MaxErrors": "1",
       "Status": "SUCCESS",
       "StartTime": 1557593493.162,
       "EndTime": 1557593498.57
   }
   ```

1. Run the following command to get the specific task invocations performed for a task execution\.

------
#### [ Linux ]

   ```
   aws ssm describe-maintenance-window-execution-task-invocations \
       --window-execution-id "14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE" \
       --task-id "c9b05aba-197f-4d8d-be34-e73fbEXAMPLE"
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-maintenance-window-execution-task-invocations ^
       --window-execution-id "14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE" ^
       --task-id "c9b05aba-197f-4d8d-be34-e73fbEXAMPLE"
   ```

------

   The system returns information like the following\.

   ```
   {
       "WindowExecutionTaskInvocationIdentities": [
           {
               "WindowExecutionId": "14bea65d-5ccc-462d-a2f3-e99c8EXAMPLE",
               "TaskExecutionId": "c9b05aba-197f-4d8d-be34-e73fbEXAMPLE",
               "InvocationId": "c336d2ab-09de-44ba-8f6a-6136cEXAMPLE",
               "ExecutionId": "76a5a04f-caf6-490c-b448-92c02EXAMPLE",
               "TaskType": "RUN_COMMAND",
               "Parameters": "{\"documentName\":\"AWS-RunShellScript\",\"instanceIds\":[\"i-02573cafcfEXAMPLE\"],\"maxConcurrency\":\"1\",\"maxErrors\":\"1\",\"parameters\":{\"commands\":[\"df\"]}}",
               "Status": "SUCCESS",
               "StatusDetails": "Success",
               "StartTime": 1557593493.222,
               "EndTime": 1557593498.466
           }
       ]
   }
   ```