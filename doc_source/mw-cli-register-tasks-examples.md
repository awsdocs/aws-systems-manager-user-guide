# Examples: Register Tasks with a Maintenance Window<a name="mw-cli-register-tasks-examples"></a>

In this topic, we provide examples of using the `register-task-with-maintenance-window` CLI command to register each of the four supported task types with a maintenance window\. The examples are for demonstration only, but you can modify them to create working task registration commands\. 

**Using the \-\-cli\-input\-json option**  
To better manage your task options, you can use the command option `--cli-input-json`, with option values referenced in a JSON file\. 

To use the sample JSON file content we provide in the following examples, do the following on your local machine:

1. Create a file with a name such as `MyRunCommandTask.json`, `MyAutomationTask.json`, or another name that you prefer\.

1. Copy the contents of our JSON sample into the file\.

1. Modify the contents of the file for your task registration, and then save the file\.

1. In the same directory where you stored the file, run the following command\. Substitute your file name for *MyFile\.json*\. 

   ```
   aws ssm register-task-with-maintenance-window --cli-input-json file://MyFile.json
   ```

**About pseudo parameters**  
In some examples, we use *pseudo parameters* as the method to pass ID information to your tasks\. For example, `{{TARGET_ID}}` is used to pass instance ID information to Automation, Lambda, and Step Functions tasks in our examples\. For more information about pseudo parameters in `--task-invocation-parameters` content, see [About Pseudo Parameters](mw-cli-register-tasks-parameters.md)\. 

**More information**  
For information about some fundamental `register-task-with-maintenance-window` options, see [About register\-task\-with\-maintenance\-windows Options](mw-cli-task-options.md)\.

For comprehensive information about command options, see the following topics:
+ [register\-task\-with\-maintenance\-window](https://docs.aws.amazon.com/cli/latest/reference/ssm/register-task-with-maintenance-window.html) in the *AWS CLI Command Reference*
+ [RegisterTaskWithMaintenanceWindow](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_RegisterTaskWithMaintenanceWindow.html) in the *AWS Systems Manager API Reference*

## Task Registration Examples<a name="task-examples"></a>

The following sections provide a sample AWS CLI command for registering a supported task type and a JSON sample that can be used with the `--cli-input-json` option\.

**Note**  
The CLI commands we provide are formatted to run from a local Linux machine\. To run them from a local Windows machine, remove the line breaks \(\\\) from the ends of the lines\. The sample JSON content format works on both Linux and Windows local machines\.

### Register a Systems Manager Run Command Task<a name="register-tasks-tutorial-run-command"></a>

**AWS CLI command:**

```
aws ssm register-task-with-maintenance-window --window-id mw-0c50858d01EXAMPLE \
--task-arn "AWS-RunShellScript" --max-concurrency 1 --max-errors 1 --priority 10 \
--targets "Key=InstanceIds,Values=i-02573cafcfEXAMPLE" --task-type "RUN_COMMAND" \
--task-invocation-parameters "{"RunCommand":{"Parameters":{"commands":["df"]}}}"
```

**JSON content to use with `--cli-input-json` file option:**

```
{
    "TaskType": "RUN_COMMAND",
    "WindowId": "mw-0c50858d01EXAMPLE",
    "Description": "My Run Command task to update SSM Agent on an instance",
    "MaxConcurrency": "1",
    "MaxErrors": "1",
    "Name": "My-Run-Command-Task",
    "Priority": 10,
    "Targets": [
        {
            "Key": "WindowTargetIds",
            "Values": [
                "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
            ]
        }
    ],
    "TaskArn": "AWS-UpdateSSMAgent",
    "TaskInvocationParameters": {
        "RunCommand": {
            "Comment": "A TaskInvocationParameters test comment",
            "NotificationConfig": {
                "NotificationArn": "arn:aws:sns:us-east-2:123456789012:my-sns-topic-name",
                "NotificationEvents": [
                    "All"
                ],
                "NotificationType": "Invocation"
            },
            "OutputS3BucketName": "my-s3-bucket-name",
            "OutputS3KeyPrefix": "my-s3-bucket-folder-name",
            "TimeoutSeconds": 3600
        }
    }
}
```

### Register a Systems Manager Automation Task<a name="register-tasks-tutorial-automation"></a>

**AWS CLI command:**

```
aws ssm register-task-with-maintenance-window --window-id "mw-0c50858d01EXAMPLE" \
--targets Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE \
--task-arn "AWS-RestartEC2Instance" \
--service-role-arn arn:aws:iam::123456789012:role/MyMaintenanceWindowServiceRole \
--task-type AUTOMATION \
--task-invocation-parameters "Automation={DocumentVersion=5,Parameters={instanceId='{{TARGET_ID}}'}}" \
--priority 0 --max-concurrency 10 --max-errors 5 --name "My-Automation-Task" \
--description "A description for my Automation task"
```

**JSON content to use with `--cli-input-json` file option:**

```
{
    "WindowId": "mw-0c50858d01EXAMPLE",
    "Targets": [
        {
            "Key": "WindowTargetIds",
            "Values": [
                "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
            ]
        }
    ],
    "TaskArn": "AWS-PatchInstanceWithRollback",
    "TaskType": "AUTOMATION",
    "MaxConcurrency": "10",
    "MaxErrors": "10",
    "TaskInvocationParameters": {
        "Automation": {
            "DocumentVersion": "1",
            "Parameters": {
                "instanceId": [
                    "{{TARGET_ID}}"
                ]
            }
        }
    }
}
```

### Register an AWS Lambda Task<a name="register-tasks-tutorial-lambda"></a>

For these examples, the user who created the Lambda function named it `SSMrestart-my-instances` and created two parameters called `targetId` and `targetType`\.

**Important**  
The IAM policy for Maintenance Windows requires that you prefix Lambda function \(or alias\) names with `SSM`\. Before you proceed to register this type of task, you must update its name in AWS Lambda to include SSM\. For example, if your Lambda function name is `MyLambdaFunction`, change it to `SSMMyLambdaFunction`\.

**AWS CLI command:**

```
aws ssm register-task-with-maintenance-window --window-id "mw-0c50858d01EXAMPLE" \
--targets "Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" --priority 2 \
--max-concurrency 10 --max-errors 5 --name "My-Lambda-Example" \
--description "A description for my LAMBDA example task" --task-type "LAMBDA" \
--task-arn "arn:aws:lambda:us-east-2:123456789012:function:serverlessrepo-SSMrestart-my-instances-C4JF9EXAMPLE" \
--task-invocation-parameters '{"Lambda":{\"Payload\":{\"targetId\":\"{{TARGET_ID}}\",\"targetType\":\"{{TARGET_TYPE}}\"},"Qualifier": "$LATEST"}}'
```

**JSON content to use with `--cli-input-json` file option:**

```
{
    "WindowId": "mw-0c50858d01EXAMPLE",
    "Targets": [
        {
            "Key": "WindowTargetIds",
            "Values": [
                "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
            ]
        }
    ],
    "TaskArn": "SSM_RestartMyInstances",
    "TaskType": "LAMBDA",
    "MaxConcurrency": "10",
    "MaxErrors": "10",
    "TaskInvocationParameters": {
        "Lambda": {
            "ClientContext": "ew0KICAi--truncated--0KIEXAMPLE",
            "Payload": "{ \"targetId\": \"{{TARGET_ID}}\", \"targetType\": \"{{TARGET_TYPE}}\" }",
            "Qualifier": "$LATEST"
        }
    },
    "Name": "My-Lambda-Task",
    "Description": "A description for my LAMBDA task",
    "Priority": 5
}
```

### Register an AWS Step Functions Task<a name="register-tasks-tutorial-step-functions"></a>

For these examples, the user who created the Step Functions state machine created a state machine named `SSMMyStateMachine` with a parameter called `targetId`\.

**Important**  
The IAM policy for Maintenance Windows requires that you prefix Step Functions state machine names with `SSM`\. Before you proceed to register this type of task, you must update its name in AWS Step Functions to include `SSM`\. For example, if your state machine name is `MyStateMachine`, change it to `SSMMyStateMachine`\.

**AWS CLI command:**

```
aws ssm register-task-with-maintenance-window --window-id "mw-0c50858d01EXAMPLE" \
--targets "Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" \
--task-arn arn:aws:states:us-east-2:123456789012:stateMachine:SSMMyStateMachine-MggiqEXAMPLE \ 
--task-type STEP_FUNCTIONS \
--task-invocation-parameters '{"StepFunctions":{"Input":"{\"targetId\":\"{{TARGET_ID}}\"}"}, "Name": "{{INVOCATION_ID}}"}' \
--priority 0 --max-concurrency 10 --max-errors 5 \
--name "My-Step-Functions-Task" --description "A description for my Step Functions task"
```

**JSON content to use with `--cli-input-json` file option:**

```
{
    "WindowId": "mw-0c50858d01EXAMPLE",
    "Targets": [
        {
            "Key": "WindowTargetIds",
            "Values": [
                "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
            ]
        }
    ],
    "TaskArn": "SSM_MyStateMachine",
    "TaskType": "STEP_FUNCTIONS",
    "MaxConcurrency": "10",
    "MaxErrors": "10",
    "TaskInvocationParameters": {
        "StepFunctions": {
            "Input": "{ \"targetId\": \"{{TARGET_ID}}\" }",
            "Name": "{{INVOCATION_ID}}"
        }
    },
    "Name": "My-Step-Functions-Task",
    "Description": "A description for my Step Functions task",
    "Priority": 5
}
```