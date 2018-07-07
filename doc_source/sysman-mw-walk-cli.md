# Walkthrough: Create and Configure a Maintenance Window \(CLI\)<a name="sysman-mw-walk-cli"></a>

The following walkthrough describes how to create and configure a Maintenance Window, targets, and tasks by using the AWS CLI\.

**To create and configure a Maintenance Window Using the AWS CLI**

1. [Download](https://aws.amazon.com/cli/) the AWS CLI to your local machine\.

1. Open the AWS CLI and run the following command to create a Maintenance Window that runs at 4 PM on every Tuesday for 4 hours, with a 1 hour cutoff, and that allows unassociated targets\. For more information about creating cron expressions for the `schedule` parameter, see [Reference: Cron and Rate Expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

   ```
   aws ssm create-maintenance-window --name "My-First-Maintenance-Window" --schedule "cron(0 16 ? * TUE *)" --duration 4 --cutoff 1 --allow-unassociated-targets
   ```

   The system returns information like the following\.

   ```
   {
      "WindowId":"mw-ab12cd34ef56gh78"
   }
   ```

1. Run the following command to list all Maintenance Windows in your AWS account\.

   ```
   aws ssm describe-maintenance-windows
   ```

   The system returns information like the following\.

   ```
   {
      "WindowIdentities":[
         {
            "Duration":4,
            "Cutoff":1,
            "WindowId":"mw-ab12cd34ef56gh78",
            "Enabled":true,
            "Name":"My-First-Maintenance-Window"
         }
      ]
   }
   ```

1. Run the following command to register an instance as a target for this Maintenance Windows\. The system returns a Maintenance Window target ID\. You will use this ID in a later step to register a task for this Maintenance Window\.

   ```
   aws ssm register-target-with-maintenance-window --window-id "mw-ab12cd34ef56gh78" --target "Key=InstanceIds,Values=ID" --owner-information "Single instance" --resource-type "INSTANCE"
   ```

   The system returns information like the following\.

   ```
   {
      "WindowTargetId":"1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d-1a2"
   }
   ```

   You could register multiple instances using the following command\.

   ```
   aws ssm register-target-with-maintenance-window --window-id "mw-ab12cd34ef56gh78" --targets "Key=InstanceIds,Values=ID 1,ID 2" --owner-information "Two instances in a list" --resource-type "INSTANCE"
   ```

   You could also register instances using EC2 tags\.

   ```
   aws ssm register-target-with-maintenance-window --window-id "mw-ab12cd34ef56gh78" --targets "Key=tag:Environment,Values=Prod" "Key=Role,Values=Web" --owner-information "Production Web Servers" --resource-type "INSTANCE"
   ```

1. Use the following command to display the targets for a Maintenance Window\.

   ```
   aws ssm describe-maintenance-window-targets --window-id "mw-ab12cd34ef56gh78"
   ```

   The system returns information like the following\.

   ```
   {
      "Targets":[
         {
            "ResourceType":"INSTANCE",
            "OwnerInformation":"Single instance",
            "WindowId":"mw-ab12cd34ef56gh78",
            "Targets":[
               {
                  "Values":[
                     "i-11aa22bb33cc44dd5"
                  ],
                  "Key":"InstanceIds"
               }
            ],
            "WindowTargetId":"a1b2c3d4-a1b2-a1b2-a1b2-a1b2c3d4"
         },
         {
            "ResourceType":"INSTANCE",
            "OwnerInformation":"Two instances in a list",
            "WindowId":"mw-ab12cd34ef56gh78",
            "Targets":[
               {
                  "Values":[
                     "i-1a2b3c4d5e6f7g8h9",
                     "i-aa11bb22cc33dd44e "
                  ],
                  "Key":"InstanceIds"
               }
            ],
            "WindowTargetId":"1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d-1a2"
         },
         {
            "ResourceType":"INSTANCE",
            "OwnerInformation":"Production Web Servers",
            "WindowId":"mw-ab12cd34ef56gh78",
            "Targets":[
               {
                  "Values":[
                     "Prod"
                  ],
                  "Key":"tag:Environment"
               },
               {
                  "Values":[
                     "Web"
                  ],
                  "Key":"tag:Role"
               }
            ],
            "WindowTargetId":"1111aaa-2222-3333-4444-1111aaa "
         }
      ]
   }
   ```

1. Run the following command to register a task for the Maintenance Window\. The task in first example uses Systems Manager Run Command to run the `df` command using the AWS\-RunShellScript document\. You can also specify tasks that use Systems Manager Automation, AWS Lambda, and AWS Step Functions, as shown in the additional examples\. You can specify the following parameters when registering a task:
   + **`targets`**: Specify either Key=WindowTargetIds,Values=*IDs* to specify a target that is already registered with the Maintenance Window\. Or, specify Key=InstanceIds,Values=*IDs* to target individual instances that may or may not be registered with the Maintenance Window\. 
   + **`task-arn`**: The resource that the task uses during execution\. For RUN\_COMMAND and AUTOMATION task types, `TaskArn` is the SSM document name or ARN\. For LAMBDA tasks, it's the function name or ARN\. For STEP\_FUNCTION tasks, it's the state machine ARN\.
   + **`window-id`**: The ID of the target Maintenance Window\.
   + **`task-type`**: The type of task\. The type can be one of the following: `RUN_COMMAND`, `AUTOMATION`, `LAMBDA`, or `STEP_FUNCTION`\.
   + **`task-invocation-parameters`**: Required and optional parameters\. Some of the common `task-invocation-parameters` parameters are described in the next list\. 
   + **`max-concurrency`**: \(Optional\) The maximum number of instances that are allowed to run the command at the same time\. You can specify a number such as 10 or a percentage such as 10%\.
   + **`max-errors`**: \(Optional\) The maximum number of errors allowed without the command failing\. When the command fails one more time beyond the value of `MaxErrors`, the systems stops sending the command to additional targets\. You can specify a number such as 10 or a percentage such as 10%\.
   + **`priority`**: The priority of the task in the Maintenance Window\. The lower the number the higher the priority \(for example, 1 is highest priority\)\. Tasks in a Maintenance Window are scheduled in priority order\. Tasks that have the same priority are scheduled in parallel\. 

   **Common parameters for task\-invocation\-parameters**

   The following list describes some of the common parameters that you can specify when using `task-invocation-parameters`\. You specify these parameters by using the `{{ PARAMETER_NAME }}` syntax, as shown in the examples in this section\.
   + **`TARGET_ID`**: The ID of the target\. If the target type is INSTANCE \(currently the only supported type\), then the target ID is the instance ID\.
   + **`TARGET_TYPE`**: The type of target\. Currently only INSTANCE is supported\.
   + **`WINDOW_ID`**: The ID of the target Maintenance Window\.
   + **`WINDOW_TASK_ID`**: The ID of the window task that is executing\.
   + **`WINDOW_TARGET_ID`**: The ID of the window target that includes the target \(target ID\)\.
   + **`LOGGING_S3_BUCKET_NAME`**: The Amazon S3 bucket name, if configured by using the `logging-info` parameter\.
   + **`LOGGING_S3_KEY_PREFIX`**: The Amazon S3 key prefix, if configured by using the `logging-info` parameter\.
   + **`LOGGING_S3_REGION`**: The Amazon S3 Region, if configured by using the `logging-info` parameter\.
   + **`WINDOW_EXECUTION_ID`**: The ID of the current window execution\.
   + **`TASK_EXECUTION_ID`**: The ID of the current task execution\.
   + **`INVOCATION_ID`**: The ID of the current invocation\.

   ```
   aws ssm register-task-with-maintenance-window --window-id mw-ab12cd34ef56gh78 --task-arn "AWS-RunShellScript" --targets "Key=InstanceIds,Values=Instance ID" --service-role-arn "arn:aws:iam::1122334455:role/MW-Role" --task-type "RUN_COMMAND" --task-invocation-parameters '{"RunCommand":{"Parameters":{"commands":["df"]}}}' --max-concurrency 1 --max-errors 1 --priority 10
   ```

   The system returns information like the following\.

   ```
   {
      "WindowTaskId":"44444444-5555-6666-7777-88888888"
   }
   ```

   You can also register a task using a Maintenance Window target ID\. The Maintenance Window target ID was returned from an earlier command\. 

   ```
   aws ssm register-task-with-maintenance-window --targets "Key=WindowTargetIds,Values=Window Target ID" --task-arn "AWS-RunShellScript" --service-role-arn "arn:aws:iam::1122334455:role/MW-Role" --window-id "mw-ab12cd34ef56gh78" --task-type "RUN_COMMAND" --task-invocation-parameters  '{"RunCommand":{"Parameters":{"commands":["df"]}}}' --max-concurrency 1 --max-errors 1 --priority 10
   ```

   The system returns information like the following\.

   ```
   {
      "WindowTaskId":"44444444-5555-6666-7777-88888888"
   }
   ```

   The following examples demonstrate how to register other task types\.
**Important**  
The IAM policy for Maintenance Windows requires that you prefix Lambda function \(or alias\) names and Step Functions state machine names with SSM, as shown in the first two examples below\. Before you proceed to register these types of tasks, you must update their names in AWS Lambda and AWS Step Functions to include SSM\.

   Lambda

   ```
   aws ssm register-task-with-maintenance-window --window-id "mw-0290d787d641f11f3" --targets Key=WindowTargetIds,Values=31547414-69c3-49f8-95b8-ed2dcf045faa --task-arn arn:aws:lambda:us-east-2:711106535523:function:SSMTestFunction --service-role-arn arn:aws:iam::711106535523:role/MaintenanceWindows --task-type LAMBDA --task-invocation-parameters '{"Lambda":{"Payload":"{\"targetId\":\"{{TARGET_ID}}\",\"targetType\":\"{{TARGET_TYPE}}\"}","Qualifier":"$LATEST","ClientContext":"ew0KICAiY3VzdG9tIjogew0KICAgICJjbGllbnQiOiAiQVdTQ0xJIg0KICB9DQp9"}}' --priority 0 --max-concurrency 10 --max-errors 5 --name "Lambda_Example" --description "My Lambda Example"
   ```

   Step Functions

   ```
   aws ssm register-task-with-maintenance-window --window-id "mw-0290d787d641f11f3" --targets Key=WindowTargetIds,Values=31547414-69c3-49f8-95b8-ed2dcf045faa --task-arn arn:aws:states:us-east-2:711106535523:stateMachine:SSMTestStateMachine --service-role-arn arn:aws:iam::711106535523:role/MaintenanceWindows --task-type STEP_FUNCTIONS --task-invocation-parameters '{"StepFunctions":{"Input":"{\"instanceId\":\"{{TARGET_ID}}\"}"}}' --priority 0 --max-concurrency 10 --max-errors 5 --name "Step_Functions_Example" --description "My Step Functions Example"
   ```

   Automation

   ```
   aws ssm register-task-with-maintenance-window --window-id "mw-0290d787d641f11f3" --targets Key=WindowTargetIds,Values=31547414-69c3-49f8-95b8-ed2dcf045faa --task-arn AutomationDocumentName --service-role-arn arn:aws:iam::711106535523:role/MaintenanceWindows --task-type AUTOMATION --task-invocation-parameters "Automation={DocumentVersion=5,Parameters={instanceId='{{TARGET_ID}}'}}" --priority 0 --max-concurrency 10 --max-errors 5 --name Name --description Description
   ```

   Run Command

   ```
   aws ssm register-task-with-maintenance-window --window-id "mw-0290d787d641f11f3" --targets Key=WindowTargetIds,Values=31547414-69c3-49f8-95b8-ed2dcf045faa --task-arn AWS-RunPowerShellScript --service-role-arn arn:aws:iam::711106535523:role/MaintenanceWindows --task-type RUN_COMMAND --task-invocation-parameters "RunCommand={Comment=SomeComment,DocumentHashType=Sha256,DocumentHash=b9d0966408047ebcafee82de4d42477299306fd37510c6815c19e9848e2bffe8,NotificationConfig={NotificationArn=arn:aws:sns:us-west-2:711106535523:RunCommandTopic,NotificationEvents=[Success,Failed],NotificationType=Invocation},OutputS3BucketName=MyS3Bucket,OutputS3KeyPrefix=RunCommand,ServiceRoleArn=arn:aws:iam::711106535523:role/RunCommand,TimeoutSeconds=30,Parameters={commands=ipconfig}}" --priority 0 --max-concurrency 10 --max-errors 5 --name "Run_Command_Sample" --description "My Run Command Sample"
   ```

1. Run the following command to list all registered tasks for a Maintenance Window\.

   ```
   aws ssm describe-maintenance-window-tasks --window-id "mw-ab12cd34ef56gh78"
   ```

   The system returns information like the following\.

   ```
   {
      "Tasks":[
         {
            "ServiceRoleArn":"arn:aws:iam::11111111:role/MW-Role",
            "MaxErrors":"1",
            "TaskArn":"AWS-RunPowerShellScript",
            "MaxConcurrency":"1",
            "WindowTaskId":"3333-3333-3333-333333",
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
                  "Values":[
                     "i-1a2b3c4d5e6f7g8h9"
                  ],
                  "Key":"InstanceIds"
               }
            ]
         },
         {
            "ServiceRoleArn":"arn:aws:iam::2222222222:role/MW-Role",
            "MaxErrors":"1",
            "TaskArn":"AWS-RunPowerShellScript",
            "MaxConcurrency":"1",
            "WindowTaskId":"44444-44-44-444444",
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
                  "Values":[
                     "555555-55555-555-5555555"
                  ],
                  "Key":"WindowTargetIds"
               }
            ]
         }
      ]
   }
   ```

1. Run the following command to view a list of task executions for a specific Maintenance Window\.

   ```
   aws ssm describe-maintenance-window-executions --window-id "mw-ab12cd34ef56gh78"
   ```

   The system returns information like the following\.

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
         # ... omitting a number of entries in the interest of space...      {
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
      "NextToken":111111   ..."
   }
   ```

1. Run the following command to get information about a Maintenance Window task execution\.

   ```
   aws ssm get-maintenance-window-execution --window-execution-id "1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d-1a2"
   ```

   The system returns information like the following\.

   ```
   {
      "Status":"SUCCESS",
      "TaskIds":[
         "333-33-3333-333333"
      ],
      "StartTime":1478230495.472,
      "EndTime":1478230516.505,
      "WindowExecutionId":"1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d-1a2"
   }
   ```

1. Run the following command to list the tasks run as part of a Maintenance Window execution\.

   ```
   aws ssm describe-maintenance-window-execution-tasks --window-execution-id "1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d-1a2"
   ```

   The system returns information like the following\.

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

1. Run the following command to get the details of a task execution\.

   ```
   aws ssm get-maintenance-window-execution-task --window-execution-id "555555-555-55-555555" --task-id "4444-4444-4444-444444"
   ```

   The system returns information like the following\.

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
   aws ssm describe-maintenance-window-execution-task-invocations --window-execution-id "555555-555-55-555555" --task-id "4444-4444-4444-444444"
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

1. If you want, run the following command to delete the Maintenance Window you created\.

   ```
   aws ssm delete-maintenance-window --window-id "mw-1a2b3c4d5e6f7g8h9"
   ```

   The system returns information like the following:

   ```
   {
      "WindowId":"mw-1a2b3c4d5e6f7g8h9"
   }
   ```