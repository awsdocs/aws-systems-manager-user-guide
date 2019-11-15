# About Pseudo Parameters<a name="mw-cli-register-tasks-parameters"></a>

When you register a task, you use the `--task-invocation-parameters` option to specify the parameters that are unique to each of the four task types\. You can also reference certain values using *pseudo parameter* syntax, such as `{{TARGET_ID}}`\. When the maintenance window task runs, it passes the correct values instead of the pseudo parameter placeholders\.

For Run Command tasks, the connections to targets \(instances\) are handled automatically\. For the other task types–Automation, Step Functions, and Lambda–you must include the `{{TARGET_ID}}` pseudo parameters in the `--task-invocation-parameters` option\. When the task runs, the actual instance ID is supplied in place of `{{TARGET_ID}}`\.

**Examples**  
For example, suppose that your payload for a Lambda task needs to reference the instance ID and target type when it runs\. If the task would only ever run on a single instance, with instance ID i\-02573cafcfEXAMPLE, you could use the following:

```
"TaskArn": "arn:aws:lambda:us-east-2:111122223333:function:SSMTestFunction",
    "TaskType": "LAMBDA",
    "TaskInvocationParameters": {
        "Lambda": {
            "Payload": "{ \"targetId\": \"i-02573cafcfEXAMPLE\", \"targetType\": \"INSTANCE\" }",
            "Qualifier": "$LATEST",
            "ClientContext": "ew0KICAi--truncated--0KIEXAMPLE"
    }
}
```

However, if you include multiple instance IDs in the custom `"targetId"` field, the operation fails\. Therefore, to run the task on multiple instances, you use the `{{TARGET_ID}}` and `{{TARGET_TYPE}}` pseudo parameters\. In this case, the ID of each instance in the target group is passed to the task execution through the `{{TARGET_ID}}` pseudo parameter, and the task runs on every instance in your target group:

```
"TaskArn": "arn:aws:lambda:us-east-2:111122223333:function:SSMTestFunction",
    "TaskType": "LAMBDA",
    "TaskInvocationParameters": {
        "Lambda": {
            "ClientContext": "ew0KICAi--truncated--0KIEXAMPLE",
            "Payload": "{ \"targetId\": \"{{TARGET_ID}}\", \"targetType\": \"{{TARGET_TYPE}}\" }",
            "Qualifier": "$LATEST"
        }
    }
```

As another example, to run an Automation task that stops your Amazon EC2 instances, you specify the `AWS-StopEC2Instance` SSM document as the `TaskArn` value and use the `{{TARGET_ID}` pseudo parameter:

```
"TaskArn": "AWS-StopEC2Instance",
    "TaskType": "AUTOMATION"
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
```

**Available pseudo parameters**  
The following list describes the pseudo parameters that you can specify using the `{{PSEUDO_PARAMETER}}` syntax in the `--task-invocation-parameters` option\.
+ **`TARGET_ID`**: The ID of the target\. If the target type is INSTANCE \(currently the only supported type\), then the target ID is the instance ID\.
+ **`TARGET_TYPE`**: The type of target\. Currently only INSTANCE is supported\.
+ **`WINDOW_ID`**: The ID of the target maintenance window\.
+ **`WINDOW_TASK_ID`**: The ID of the window task that is executing\.
+ **`WINDOW_TARGET_ID`**: The ID of the window target that includes the target \(target ID\)\.
+ **`WINDOW_EXECUTION_ID`**: The ID of the current window execution\.
+ **`TASK_EXECUTION_ID`**: The ID of the current task execution\.
+ **`INVOCATION_ID`**: The ID of the current invocation\.