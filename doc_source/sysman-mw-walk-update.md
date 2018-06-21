# Walkthrough: Update a Maintenance Window<a name="sysman-mw-walk-update"></a>

This section describes how to update a Maintenance Window by using the AWS CLI\. This section also includes information updating different task types, including Systems Manager Run Command, Systems Manager Automation, AWS Lambda, and AWS Step Functions tasks\. For more information about updating a Maintenance Window, see [Update or Delete a Maintenance Window](sysman-maintenance-update.md)\. 

The examples in this section use the following Systems Manager actions for updating a Maintenance Window\.
+ [UpdateMaintenanceWindow](http://docs.aws.amazon.com/systems-manager/latest/APIReference/UpdateMaintenanceWindow.html)
+ [UpdateMaintenanceWindowTarget](http://docs.aws.amazon.com/systems-manager/latest/APIReference/UpdateMaintenanceWindowTarget.html)
+ [UpdateMaintenanceWindowTask](http://docs.aws.amazon.com/systems-manager/latest/APIReference/UpdateMaintenanceWindowTask.html)
+ [DeregisterTargetFromMaintenanceWindow](http://docs.aws.amazon.com/systems-manager/latest/APIReference/DeregisterTargetFromMaintenanceWindow.html)

**To update a Maintenance Window**

1. Run the following command to update a target to include a name and a description\.

   ```
   aws ssm update-maintenance-window-target --window-id "mw-12345678910" --window-target-id "a1b2c3d4-e5f6-g7h8i9" --name "NewTargetName" --description "NewTargetName description"
   ```

   The system returns information like the following\.

   ```
   {
       "WindowId": "mw-12345678910",
       "WindowTargetId": "a1b2c3d4-e5f6-g7h8i9",
       "Targets": [
           {
               "Key": "InstanceIds",
               "Values": [
                   "i-aabbccddeeff"
               ]
           }
       ],
       "Name": "NewTargetName",
       "Description": "NewTargetName description"
   }
   ```

1. Run the following command to use the `replace` option to remove the description field and add an additional target\. The description field is removed, because the update does not include the field \(a null value\)\.

   ```
   aws ssm update-maintenance-window-target --window-id "mw-12345678910" --window-target-id "a1b2c3d4-e5f6-g7h8i9" --targets "Key=InstanceIds,Values=i-aabbccddeeff,i-223344556677" --name "NewTargetName" --replace
   ```

   The system returns information like the following\.

   ```
   {
       "WindowId": "mw-12345678910",
       "WindowTargetId": "a1b2c3d4-e5f6-g7h8i9",
       "Targets": [
           {
               "Key": "InstanceIds",
               "Values": [
                   "i-aabbccddeeff",
                   "i-223344556677"
               ]
           }
       ],
       "Name": "NewTargetName"
   }
   ```

1. Run the following command to update a Run Command task\.

   ```
   aws ssm update-maintenance-window-task --window-id "mw-12345678910" --window-task-id "1111-2222-3333-4444-5555" --targets "Key=WindowTargetIds,Values=a1b2c3d4-e5f6-g7h8i9c" --task-arn "AWS-RunPowerShellScript" --service-role-arn "arn:aws:iam::abcdefghijk:role/MaintenanceWindowsRole" --task-invocation-parameters "RunCommand={Comment=A_Comment,Parameters={commands=ipconfig}}" --priority 1 --max-concurrency 10 --max-errors 4 --name "RC_Name" --description "RC_Name description extra"
   ```

   The system returns information like the following\.

   ```
   {
       "WindowId": "mw-12345678916",
       "WindowTaskId": "aaa-bbb-ccc-ddd",
       "Targets": [
           {
               "Key": "WindowTargetIds",
               "Values": [
                   "a1b2c3d4-e5f6-g7h8i9c"
               ]
           }
       ],
       "TaskArn": "AWS-RunPowerShellScript",
       "ServiceRoleArn": "arn:aws:iam::abcdefghijk:role/MaintenanceWindowsRole",
       "TaskParameters": {},
       "TaskInvocationParameters": {
           "RunCommand": {
               "Comment": "SomeComment",
               "Parameters": {
                   "commands": [
                       "ipconfig -tail"
                   ]
               }
           }
       },
       "Priority": 1,
       "MaxConcurrency": "10",
       "MaxErrors": "4",
       "Name": "RC_Name",
       "Description": "RC_Name description extra"
   }
   ```

1. Run the following command to add a name and a description to a Lambda task\.

   ```
   aws ssm update-maintenance-window-task --window-id mw-1234567891 --window-task-id 1a2b3c4d-5e6f-7g8h90 --targets "Key=WindowTargetIds,Values=a1b2c3d4-e5f6-g7h8i9c,4444-555555-66666-7777" --task-arn "arn:aws:lambda:us-east-2:1313131313:function:SSMTestLambda" --service-role-arn "arn:aws:iam::abcdefghijk:role/MaintenanceWindowsRole" --task-invocation-parameters '{"Lambda":{"Payload":"{\"targetId\":\"{{TARGET_ID}}\",\"targetType\":\"{{TARGET_TYPE}}\"}"}}' --priority 0 --max-concurrency 10 --max-errors 5 --name "TestLambda_Name" --description "My Rename Test"
   ```

   The system returns information like the following\.

   ```
   {
       "WindowId": "mw-1234567891",
       "WindowTaskId": "1a2b3c4d-5e6f-7g8h90",
       "Targets": [
           {
               "Key": "WindowTargetIds",
               "Values": [
                   "a1b2c3d4-e5f6-g7h8i9c",
                   "4444-555555-66666-7777d"
               ]
           }
       ],
       "TaskArn": "arn:aws:lambda:us-east-2:1313131313:function:SSMTestLambda",
       "ServiceRoleArn": "arn:aws:iam::abcdefghijk:role/MaintenanceWindowsRole",
       "TaskParameters": {},
       "TaskInvocationParameters": {
           "Lambda": {
               "Payload": "e30="
           }
       },
       "Priority": 0,
       "MaxConcurrency": "10",
       "MaxErrors": "5",
       "Name": "TestLambda_Name",
       "Description": "TestLambda_Name description"
   }
   ```

1. Run the following command to update an AWS Step Functions task to update task\-invocation\-parameters\.

   ```
   aws ssm update-maintenance-window-task --window-id "mw-1234567891" --window-task-id "1a2b3c4d-5e6f-7g8h9i" --targets "Key=WindowTargetIds,Values=a1b2c3d4-e5f6-g7h8i9c" --task-arn "arn:aws:states:us-east-2:4242424242:execution:SSMStepFunctionTest" --service-role-arn "arn:aws:iam::abcdefghijk:role/MaintenanceWindowsRole" --task-invocation-parameters '{"StepFunctions":{"Input":"{\"instanceId\":\"{{ TARGET_ID }}\"}"}}' --priority 0 --max-concurrency 10 --max-errors 5 --name "Update_Parameters" --description "Test to update task invocation parameters"
   ```

   The system returns information like the following\.

   ```
   {
       "WindowId": "mw-1234567891",
       "WindowTaskId": "1a2b3c4d-5e6f-7g8h9i",
       "Targets": [
           {
               "Key": "WindowTargetIds",
               "Values": [
                   "a1b2c3d4-e5f6-g7h8i9c"
               ]
           }
       ],
       "TaskArn": "arn:aws:states:us-east-2:4242424242:execution:SSMStepFunctionTest",
       "ServiceRoleArn": "arn:aws:iam::abcdefghijk:role/MaintenanceWindowsRole",
       "TaskParameters": {},
       "TaskInvocationParameters": {
           "StepFunctions": {
               "Input": "{\"instanceId\":\"{{ TARGET_ID }}\"}"
           }
       },
       "Priority": 0,
       "MaxConcurrency": "10",
       "MaxErrors": "5",
       "Name": "TestStepFunction_Task",
       "Description": "TestStepFunction_Task description"
   }
   ```

1. Run the following command to unregister a target from a Maintenance Window\. This example uses the `safe` parameter to determine if the target is referenced by any tasks and therefore safe to unregister\.

   ```
   aws ssm deregister-target-from-maintenance-window --window-id "mw-1234567891b" --window-target-id "aaaa-bbbb-cccc-dddd" --safe
   ```

   The system returns information like the following\.

   ```
   An error occurred (TargetInUseException) when calling the DeregisterTargetFromMaintenanceWindow operation: This Target cannot be deregistered because it is still referenced in Task: a11b22c33d44e55f66
   ```

1. Run the following command to unregister a target from a Maintenance Window even if the target is referenced by a task\. You can force the unregister operation by using the `no-safe` parameter\.

   ```
   aws ssm deregister-target-from-maintenance-window --window-id "mw-1234567891b" --window-target-id "aaaa-bbbb-cccc-dddd" --no-safe 
   ```

   The system returns information like the following\.

   ```
   {
       "WindowId": "mw-1234567891b",
       "WindowTargetId": "aaaa-bbbb-cccc-ddd"
   }
   ```

1. Run the following command to update a Run Command task\. This example uses a Systems Manager Parameter Store parameter called UpdateLevel, which is formated as follows:'\{\{ssm:UpdateLevel\}\}'

   ```
   aws ssm update-maintenance-window-task --window-id "mw-1234567891b" --window-task-id "777-8888-9999-0000" --targets "Key=InstanceIds,Values=i-yyyyzzzzxxx111222"  --task-invocation-parameters "RunCommand={Comment=SomeComments,Parameters={UpdateLevel='{{ssm:UpdateLevel}}'}}"
   ```

   The system returns information like the following\.

   ```
   {
       "WindowId": "mw-1234567891b",
       "WindowTaskId": "777-8888-9999-0000",
       "Targets": [
           {
               "Key": "InstanceIds",
               "Values": [
                   "i-yyyyzzzzxxx1112223"
               ]
           }
       ],
       "TaskArn": "AWS-InstallMissingWindowsUpdates",
       "ServiceRoleArn": "arn:aws:iam::abcdefghijk:role/MaintenanceWindows",
       "TaskParameters": {},
       "TaskInvocationParameters": {
           "RunCommand": {
               "Comment": "SomeComments",
               "Parameters": {
                   "UpdateLevel": [
                       "{{ssm:UpdateLevel}}"
                   ]
               }
           }
       },
       "Priority": 0,
       "MaxConcurrency": "10",
       "MaxErrors": "5",
       "Name": "TracyMWTest_RunCommand2",
       "Description": "Test_RunCommandandParameterStore description"
   }
   ```

1. Run the following command to update an Automation task to specify WINDOW\_ID and WINDOW\_TASK\_ID parameters for the `task-invocation-parameters` parameter\.

   ```
   aws ssm update-maintenance-window-task --window-id "mw-1234567891b" --window-task-id "777-8888-9999-000" --targets "Key=WindowTargetIds,Values=999-aaa-888-bbb-777 --task-arn "AutoTestDoc" --service-role-arn arn:aws:iam::801422537783:role/MaintenanceWindowsRoleTesting --task-invocation-parameters "Automation={Parameters={instanceId='{{TARGET_ID}}',initiator='{{WINDOW_ID}}.Task-{{WINDOW_TASK_ID}}'}}" --priority 0 --max-concurrency 10 --max-errors 5
   ```

   The system returns information like the following\.

   ```
   {
       "WindowId": "mw-0a097ccb2abd5775b",
       "WindowTaskId": "777-8888-9999-0000",
       "Targets": [
           {
               "Key": "WindowTargetIds",
               "Values": [
                   "999-aaa-888-bbb-777"
               ]
           }
       ],
       "TaskArn": "AutoTestDoc",
       "ServiceRoleArn": "arn:aws:iam::abcdefghijk:role/MaintenanceWindows",
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
       "Name": "TestAutomation_Task",
       "Description": "TestAutomation_Task description"
   }
   ```