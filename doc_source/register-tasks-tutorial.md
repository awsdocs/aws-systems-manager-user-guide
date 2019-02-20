# Create a Maintenance Window Task \(AWS CLI\)<a name="register-tasks-tutorial"></a>

1. Run the following command to register a task for the Maintenance Window\. The task in first example uses Systems Manager Run Command to run the `df` command using the `AWS-RunShellScript` document\. You can also specify tasks that use Systems Manager Automation, AWS Lambda, and AWS Step Functions, as shown in the additional examples\. For information about the options you can specify, see [About 'register\-task\-with\-maintenance\-window' Options and Values](register-tasks-options.md)\.

   ```
   aws ssm register-task-with-maintenance-window --window-id mw-0c5ed765acEXAMPLE --task-arn "AWS-RunShellScript" --targets "Key=InstanceIds,Values=i-456jkl321EXAMPLE" --service-role-arn "arn:aws:iam::1122334455:role/MW-Role" --task-type "RUN_COMMAND" --task-invocation-parameters '{"RunCommand":{"Parameters":{"commands":["df"]}}}' --max-concurrency 1 --max-errors 1 --priority 10
   ```

   The system returns information like the following:

   ```
   {
      "WindowTaskId":"44444444-5555-6666-7777-88888888"
   }
   ```

   You can also register a task using a Maintenance Window target ID\. The Maintenance Window target ID was returned from an earlier command\. 

   ```
   aws ssm register-task-with-maintenance-window --targets "Key=WindowTargetIds,Values=5d2b9275-40f7-40e8-b831-95136EXAMPLE" --task-arn "AWS-RunShellScript" --service-role-arn "arn:aws:iam::1122334455:role/MW-Role" --window-id "mw-0c5ed765acEXAMPLE" --task-type "RUN_COMMAND" --task-invocation-parameters  '{"RunCommand":{"Parameters":{"commands":["df"]}}}' --max-concurrency 1 --max-errors 1 --priority 10
   ```

   The system returns information like the following:

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
   aws ssm register-task-with-maintenance-window --window-id "mw-0c5ed765acEXAMPLE" --targets Key=WindowTargetIds,Values=31547414-69c3-49f8-95b8-ed2dcEXAMPLE --task-arn arn:aws:lambda:us-east-2:111122223333:function:SSMTestFunction --service-role-arn arn:aws:iam::111122223333:role/MaintenanceWindows --task-type LAMBDA --task-invocation-parameters '{"Lambda":{"Payload":"{\"targetId\":\"{{TARGET_ID}}\",\"targetType\":\"{{TARGET_TYPE}}\"}","Qualifier":"$LATEST","ClientContext":"ew0KICAiY3VzdG9tIjogew0KICAgICJjbGllbnQiOiAiQVdTQ0xJIg0KIEXAMPLE"}}' --priority 0 --max-concurrency 10 --max-errors 5 --name "Lambda_Example" --description "My Lambda Example"
   ```

   Step Functions

   ```
   aws ssm register-task-with-maintenance-window --window-id "mw-0c5ed765acEXAMPLE" --targets Key=WindowTargetIds,Values=31547414-69c3-49f8-95b8-ed2dcEXAMPLE --task-arn arn:aws:states:us-east-2:111122223333:stateMachine:SSMTestStateMachine --service-role-arn arn:aws:iam::111122223333:role/MaintenanceWindows --task-type STEP_FUNCTIONS --task-invocation-parameters '{"StepFunctions":{"Input":"{\"instanceId\":\"{{TARGET_ID}}\"}"}}' --priority 0 --max-concurrency 10 --max-errors 5 --name "Step_Functions_Example" --description "My Step Functions Example"
   ```

   Automation

   ```
   aws ssm register-task-with-maintenance-window --window-id "mw-0c5ed765acEXAMPLE" --targets Key=WindowTargetIds,Values=31547414-69c3-49f8-95b8-ed2dcEXAMPLE --task-arn automationdocument-name --service-role-arn arn:aws:iam::111122223333:role/MaintenanceWindows --task-type AUTOMATION --task-invocation-parameters "Automation={DocumentVersion=5,Parameters={instanceId='{{TARGET_ID}}'}}" --priority 0 --max-concurrency 10 --max-errors 5 --name "Name" --description "Description"
   ```

   Run Command

   ```
   aws ssm register-task-with-maintenance-window --window-id "mw-0c5ed765acEXAMPLE" --targets Key=WindowTargetIds,Values=31547414-69c3-49f8-95b8-ed2dcEXAMPLE --task-arn AWS-RunPowerShellScript --service-role-arn arn:aws:iam::111122223333:role/MaintenanceWindows --task-type RUN_COMMAND --task-invocation-parameters "RunCommand={Comment=SomeComment,DocumentHashType=Sha256,DocumentHash=b9d0966408047ebcafee82de4d42477299306fd37510c6815c19e9848EXAMPLE,NotificationConfig={NotificationArn=arn:aws:sns:us-west-2:111122223333:RunCommandTopic,NotificationEvents=[Success,Failed],NotificationType=Invocation},OutputS3BucketName=MyS3Bucket,OutputS3KeyPrefix=RunCommand,ServiceRoleArn=arn:aws:iam::111122223333:role/RunCommand,TimeoutSeconds=30,Parameters={commands=ipconfig}}" --priority 0 --max-concurrency 10 --max-errors 5 --name "Run_Command_Sample" --description "My Run Command Sample"
   ```

1. Run the following command to list all registered tasks for a Maintenance Window\.

   ```
   aws ssm describe-maintenance-window-tasks --window-id "mw-0c5ed765acEXAMPLE"
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
                     "i-1234567890EXAMPLE"
                  ],
                  "Key":"InstanceIds"
               }
            ]
         },
         {
            "ServiceRoleArn":"arn:aws:iam::111122223333:role/MW-Role",
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