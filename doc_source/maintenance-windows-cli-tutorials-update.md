# Tutorial: Update a maintenance window \(AWS CLI\)<a name="maintenance-windows-cli-tutorials-update"></a>

This tutorial demonstrates how to use the AWS CLI to update a maintenance window\. It also shows you how to update different task types, including those for Systems Manager Run Command, Systems Manager Automation, AWS Lambda, and AWS Step Functions\. 

The examples in this section use the following Systems Manager actions for updating a maintenance window\.
+ [UpdateMaintenanceWindow](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateMaintenanceWindow.html)
+ [UpdateMaintenanceWindowTarget](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateMaintenanceWindowTarget.html)
+ [UpdateMaintenanceWindowTask](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateMaintenanceWindowTask.html)
+ [DeregisterTargetFromMaintenanceWindow](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeregisterTargetFromMaintenanceWindow.html)

For information about using the Systems Manager console to update a maintenance window, see [Update or delete a maintenance window \(console\)](sysman-maintenance-update.md)\. 

As you follow the steps in this tutorial, replace the values in italicized *red* text with your own options and IDs\. For example, replace the maintenance window ID *mw\-0c50858d01EXAMPLE* and the instance ID *i\-02573cafcfEXAMPLE* with IDs of resources you create\.

**To update a maintenance window \(AWS CLI\)**

1. Open the AWS CLI and run the following command to update a target to include a name and a description:

------
#### [ Linux ]

   ```
   aws ssm update-maintenance-window-target \
       --window-id "mw-0c50858d01EXAMPLE" \
       --window-target-id "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" \
       --name "My-Maintenance-Window-Target" \
       --description "Description for my maintenance window target"
   ```

------
#### [ Windows ]

   ```
   aws ssm update-maintenance-window-target ^
       --window-id "mw-0c50858d01EXAMPLE" ^
       --window-target-id "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" ^
       --name "My-Maintenance-Window-Target" ^
       --description "Description for my maintenance window target"
   ```

------

   The system returns information like the following:

   ```
   {
       "WindowId": "mw-0c50858d01EXAMPLE",
       "WindowTargetId": "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE",
       "Targets": [
           {
               "Key": "InstanceIds",
               "Values": [
                   "i-02573cafcfEXAMPLE"
               ]
           }
       ],
       "Name": "My-Maintenance-Window-Target",
       "Description": "Description for my maintenance window target"
   }
   ```

1. Run the following command to use the `replace` option to remove the description field and add an additional target\. The description field is removed, because the update does not include the field \(a null value\)\. Be sure to specify an additional instance that has been configured for use with Systems Manager:

------
#### [ Linux ]

   ```
   aws ssm update-maintenance-window-target \
       --window-id "mw-0c50858d01EXAMPLE" \
       --window-target-id "d208dedf-3f6b-41ff-ace8-8e751EXAMPLE" \
       --targets "Key=InstanceIds,Values=i-02573cafcfEXAMPLE,i-0471e04240EXAMPLE" \
       --name "My-Maintenance-Window-Target" \
       --replace
   ```

------
#### [ Windows ]

   ```
   aws ssm update-maintenance-window-target ^
       --window-id "mw-0c50858d01EXAMPLE" ^
       --window-target-id "d208dedf-3f6b-41ff-ace8-8e751EXAMPLE" ^
       --targets "Key=InstanceIds,Values=i-02573cafcfEXAMPLE,i-0471e04240EXAMPLE" ^
       --name "My-Maintenance-Window-Target" ^
       --replace
   ```

------

   The system returns information like the following:

   ```
   {
       "WindowId": "mw-0c50858d01EXAMPLE",
       "WindowTargetId": "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE",
       "Targets": [
           {
               "Key": "InstanceIds",
               "Values": [
                   "i-02573cafcfEXAMPLE",
                   "i-0471e04240EXAMPLE"
               ]
           }
       ],
       "Name": "My-Maintenance-Window-Target"
   }
   ```

1. The `start-date` option allows you to delay activation of a maintenance window until a specified future date\. The `end-date` option allows you to set a date and time in the future after which the maintenance window no longer runs\. Specify the options in ISO\-8601 Extended format\.

   Run the following command to specify a date and time range for regularly scheduled maintenance window executions:

------
#### [ Linux ]

   ```
   aws ssm update-maintenance-window \
       --window-id "mw-0c50858d01EXAMPLE" \
       --start-date "2020-10-01T10:10:10Z" \
       --end-date "2020-11-01T10:10:10Z"
   ```

------
#### [ Windows ]

   ```
   aws ssm update-maintenance-window ^
       --window-id "mw-0c50858d01EXAMPLE" ^
       --start-date "2020-10-01T10:10:10Z" ^
       --end-date "2020-11-01T10:10:10Z"
   ```

------

1. Run the following command to update a Run Command task\.
**Tip**  
If your target is an EC2 instance for Windows Server, change `df` to `ipconfig`, and `AWS-RunShellScript` to `AWS-RunPowerShellScript` in the following command\.

------
#### [ Linux ]

   ```
   aws ssm update-maintenance-window-task \
       --window-id "mw-0c50858d01EXAMPLE" \
       --window-task-id "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE" \
       --targets "Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" \
       --task-arn "AWS-RunShellScript" \
       --service-role-arn "arn:aws:iam::111122223333:role/MaintenanceWindowsRole" \
       --task-invocation-parameters "RunCommand={Comment=Revising my Run Command task,Parameters={commands=df}}" \
       --priority 1 --max-concurrency 10 --max-errors 4 \
       --name "My-Task-Name" --description "A description for my Run Command task"
   ```

------
#### [ Windows ]

   ```
   aws ssm update-maintenance-window-task ^
       --window-id "mw-0c50858d01EXAMPLE" ^
       --window-task-id "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE" ^
       --targets "Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" ^
       --task-arn "AWS-RunShellScript" ^
       --service-role-arn "arn:aws:iam::111122223333:role/MaintenanceWindowsRole" ^
       --task-invocation-parameters "RunCommand={Comment=Revising my Run Command task,Parameters={commands=df}}" ^
       --priority 1 --max-concurrency 10 --max-errors 4 ^
       --name "My-Task-Name" --description "A description for my Run Command task"
   ```

------

   The system returns information like the following:

   ```
   {
       "WindowId": "mw-0c50858d01EXAMPLE",
       "WindowTaskId": "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE",
       "Targets": [
           {
               "Key": "WindowTargetIds",
               "Values": [
                   "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
               ]
           }
       ],
       "TaskArn": "AWS-RunShellScript",
       "ServiceRoleArn": "arn:aws:iam::111122223333:role/MaintenanceWindowsRole",
       "TaskParameters": {},
       "TaskInvocationParameters": {
           "RunCommand": {
               "Comment": "Revising my Run Command task",
               "Parameters": {
                   "commands": [
                       "df"
                   ]
               }
           }
       },
       "Priority": 1,
       "MaxConcurrency": "10",
       "MaxErrors": "4",
       "Name": "My-Task-Name",
       "Description": "A description for my Run Command task"
   }
   ```

1. Adapt and run the following command to update a Lambda task\.

------
#### [ Linux ]

   ```
   aws ssm update-maintenance-window-task \
       --window-id mw-0c50858d01EXAMPLE \
       --window-task-id 4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE \
       --targets "Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" \
       --task-arn "arn:aws:lambda:us-east-2:111122223333:function:SSMTestLambda" \
       --service-role-arn "arn:aws:iam::111122223333:role/MaintenanceWindowsRole" \
       --task-invocation-parameters '{"Lambda":{"Payload":"{\"instanceId\":\"{{RESOURCE_ID}}\",\"targetType\":\"{{TARGET_TYPE}}\"}"}}' \
       --priority 1 --max-concurrency 10 --max-errors 5 \
       --name "New-Lambda-Task-Name" \
       --description "A description for my Lambda task"
   ```

------
#### [ Windows ]

   ```
   aws ssm update-maintenance-window-task ^
       --window-id mw-0c50858d01EXAMPLE ^
       --window-task-id 4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE ^
       --targets "Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" ^
       --task-arn "arn:aws:lambda:us-east-2:111122223333:function:SSMTestLambda" ^
       --service-role-arn "arn:aws:iam::111122223333:role/MaintenanceWindowsRole" ^
       --task-invocation-parameters '{"Lambda":{"Payload":"{\"instanceId\":\"{{RESOURCE_ID}}\",\"targetType\":\"{{TARGET_TYPE}}\"}"}}' ^
       --priority 1 --max-concurrency 10 --max-errors 5 ^
       --name "New-Lambda-Task-Name" ^
       --description "A description for my Lambda task"
   ```

------

   The system returns information like the following:

   ```
   {
       "WindowId": "mw-0c50858d01EXAMPLE",
       "WindowTaskId": "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE",
       "Targets": [
           {
               "Key": "WindowTargetIds",
               "Values": "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
           }
       ],
       "TaskArn": "arn:aws:lambda:us-east-2:111122223333:function:SSMTestLambda",
       "ServiceRoleArn": "arn:aws:iam::111122223333:role/MaintenanceWindowsRole",
       "TaskParameters": {},
       "TaskInvocationParameters": {
           "Lambda": {
               "Payload": "e30="
           }
       },
       "Priority": 1,
       "MaxConcurrency": "10",
       "MaxErrors": "5",
       "Name": "New-Lambda-Task-Name",
       "Description": "A description for my Lambda task"
   }
   ```

1. If you are updating an AWS Step Functions task, adapt and run the following command to update its task\-invocation\-parameters:

------
#### [ Linux ]

   ```
   aws ssm update-maintenance-window-task \
       --window-id "mw-0c50858d01EXAMPLE" \
       --window-task-id "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE" \
       --targets "Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" \
       --task-arn "arn:aws:states:us-east-2:111122223333:execution:SSMStepFunctionTest" \
       --service-role-arn "arn:aws:iam::111122223333:role/MaintenanceWindowsRole" \
       --task-invocation-parameters '{"StepFunctions":{"Input":"{\"instanceId\":\"{{RESOURCE_ID}}\"}"}}' \
       --priority 0 --max-concurrency 10 --max-errors 5 \
       --name "My-Step-Functions-Task" \
       --description "A description for my Step Functions task"
   ```

------
#### [ Windows ]

   ```
   aws ssm update-maintenance-window-task ^
       --window-id "mw-0c50858d01EXAMPLE" ^
       --window-task-id "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE" ^
       --targets "Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" ^
       --task-arn "arn:aws:states:us-east-2:111122223333:execution:SSMStepFunctionTest" ^
       --service-role-arn "arn:aws:iam::111122223333:role/MaintenanceWindowsRole" ^
       --task-invocation-parameters '{"StepFunctions":{"Input":"{\"instanceId\":\"{{RESOURCE_ID}}\"}"}}' ^
       --priority 0 --max-concurrency 10 --max-errors 5 ^
       --name "My-Step-Functions-Task" ^
       --description "A description for my Step Functions task"
   ```

------

   The system returns information like the following:

   ```
   {
       "WindowId": "mw-0c50858d01EXAMPLE",
       "WindowTaskId": "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE",
       "Targets": [
           {
               "Key": "WindowTargetIds",
               "Values": [
                   "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
               ]
           }
       ],
       "TaskArn": "arn:aws:states:us-east-2:111122223333:execution:SSMStepFunctionTest",
       "ServiceRoleArn": "arn:aws:iam::111122223333:role/MaintenanceWindowsRole",
       "TaskParameters": {},
       "TaskInvocationParameters": {
           "StepFunctions": {
               "Input": "{\"instanceId\":\"{{RESOURCE_ID}}\"}"
           }
       },
       "Priority": 0,
       "MaxConcurrency": "10",
       "MaxErrors": "5",
       "Name": "My-Step-Functions-Task",
       "Description": "A description for my Step Functions task"
   }
   ```

1. Run the following command to unregister a target from a maintenance window\. This example uses the `safe` parameter to determine if the target is referenced by any tasks and therefore safe to unregister:

------
#### [ Linux ]

   ```
   aws ssm deregister-target-from-maintenance-window \
       --window-id "mw-0c50858d01EXAMPLE" \
       --window-target-id "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" \
       --safe
   ```

------
#### [ Windows ]

   ```
   aws ssm deregister-target-from-maintenance-window ^
       --window-id "mw-0c50858d01EXAMPLE" ^
       --window-target-id "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" ^
       --safe
   ```

------

   The system returns information like the following:

   ```
   An error occurred (TargetInUseException) when calling the DeregisterTargetFromMaintenanceWindow operation: 
   This Target cannot be deregistered because it is still referenced in Task: 4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE
   ```

1. Run the following command to unregister a target from a maintenance window even if the target is referenced by a task\. You can force the unregister operation by using the `no-safe` parameter:

------
#### [ Linux ]

   ```
   aws ssm deregister-target-from-maintenance-window \
       --window-id "mw-0c50858d01EXAMPLE" \
       --window-target-id "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" \
       --no-safe
   ```

------
#### [ Windows ]

   ```
   aws ssm deregister-target-from-maintenance-window ^
       --window-id "mw-0c50858d01EXAMPLE" ^
       --window-target-id "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" ^
       --no-safe
   ```

------

   The system returns information like the following:

   ```
   {
       "WindowId": "mw-0c50858d01EXAMPLE",
       "WindowTargetId": "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
   }
   ```

1. Run the following command to update a Run Command task\. This example uses a Systems Manager Parameter Store parameter called `UpdateLevel`, which is formatted as follows: '`{{ssm:UpdateLevel}}`'

------
#### [ Linux ]

   ```
   aws ssm update-maintenance-window-task \
       --window-id "mw-0c50858d01EXAMPLE" \
       --window-task-id "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE" \
       --targets "Key=InstanceIds,Values=i-02573cafcfEXAMPLE"  \
       --task-invocation-parameters "RunCommand={Comment=A comment for my task update,Parameters={UpdateLevel='{{ssm:UpdateLevel}}'}}"
   ```

------
#### [ Windows ]

   ```
   aws ssm update-maintenance-window-task ^
       --window-id "mw-0c50858d01EXAMPLE" ^
       --window-task-id "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE" ^
       --targets "Key=InstanceIds,Values=i-02573cafcfEXAMPLE"  ^
       --task-invocation-parameters "RunCommand={Comment=A comment for my task update,Parameters={UpdateLevel='{{ssm:UpdateLevel}}'}}"
   ```

------

   The system returns information like the following:

   ```
   {
       "WindowId": "mw-0c50858d01EXAMPLE",
       "WindowTaskId": "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE",
       "Targets": [
           {
               "Key": "InstanceIds",
               "Values": [
                   "i-02573cafcfEXAMPLE"
               ]
           }
       ],
       "TaskArn": "AWS-RunShellScript",
       "ServiceRoleArn": "arn:aws:iam::111122223333:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM",
       "TaskParameters": {},
       "TaskInvocationParameters": {
           "RunCommand": {
               "Comment": "A comment for my task update",
               "Parameters": {
                   "UpdateLevel": [
                       "{{ssm:UpdateLevel}}"
                   ]
               }
           }
       },
       "Priority": 10,
       "MaxConcurrency": "1",
       "MaxErrors": "1"
   }
   ```

1. Run the following command to update an Automation task to specify WINDOW\_ID and WINDOW\_TASK\_ID parameters for the `task-invocation-parameters` parameter:

------
#### [ Linux ]

   ```
   aws ssm update-maintenance-window-task \
       --window-id "mw-0c50858d01EXAMPLE" \
       --window-task-id "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE" \
       --targets "Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE \
       --task-arn "AutoTestDoc" \
       --service-role-arn arn:aws:iam::111122223333:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM \
       --task-invocation-parameters "Automation={Parameters={instanceId='{{RESOURCE_ID}}',initiator='{{WINDOW_ID}}.Task-{{WINDOW_TASK_ID}}'}}" \
       --priority 3 --max-concurrency 10 --max-errors 5
   ```

------
#### [ Windows ]

   ```
   aws ssm update-maintenance-window-task ^
       --window-id "mw-0c50858d01EXAMPLE" ^
       --window-task-id "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE" ^
       --targets "Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE ^
       --task-arn "AutoTestDoc" ^
       --service-role-arn arn:aws:iam::111122223333:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM ^
       --task-invocation-parameters "Automation={Parameters={instanceId='{{RESOURCE_ID}}',initiator='{{WINDOW_ID}}.Task-{{WINDOW_TASK_ID}}'}}" ^
       --priority 3 --max-concurrency 10 --max-errors 5
   ```

------

   The system returns information like the following:

   ```
   {
       "WindowId": "mw-0c50858d01EXAMPLE",
       "WindowTaskId": "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE",
       "Targets": [
           {
               "Key": "WindowTargetIds",
               "Values": [
                   "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
               ]
           }
       ],
       "TaskArn": "AutoTestDoc",
       "ServiceRoleArn": "arn:aws:iam::111122223333:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM",
       "TaskParameters": {},
       "TaskInvocationParameters": {
           "Automation": {
               "Parameters": {
                   "multi": [
                       "{{WINDOW_TASK_ID}}"
                   ],
                   "single": [
                       "{{WINDOW_ID}}"
                   ]
               }
           }
       },
       "Priority": 0,
       "MaxConcurrency": "10",
       "MaxErrors": "5",
       "Name": "My-Automation-Task",
       "Description": "A description for my Automation task"
   }
   ```