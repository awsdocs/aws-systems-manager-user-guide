# About pseudo parameters<a name="mw-cli-register-tasks-parameters"></a>

When you register a task, you use the `--task-invocation-parameters` option to specify the parameters that are unique to each of the four task types\. You can also reference certain values using *pseudo parameter* syntax, such as `{{RESOURCE_ID}}`, `{{TARGET_TYPE}}`, and `{{WINDOW_TARGET_ID}}`\. When the maintenance window task runs, it passes the correct values instead of the pseudo parameter placeholders\. The full list of pseudo parameters you can use is provided in [Supported pseudo parameters](#pseudo-parameters)\.

**Important**  
For the target type `RESOURCE_GROUP`, depending on the ID format needed for the task, you can choose between using `{{TARGET_ID}}` and `{{RESOURCE_ID}}` to reference the resource when your task runs\. `{{TARGET_ID}}` returns the full ARN of the resource\. `{{RESOURCE_ID}}` returns only a shorter name or ID of the resource, as shown in these examples\.  
`{{TARGET_ID}}` format: `arn:aws:ec2:us-east-1:123456789012:instance/i-02573cafcfEXAMPLE`
`{{RESOURCE_ID}}` format: `i-02573cafcfEXAMPLE`
For target type `INSTANCE`, both the `{{TARGET_ID}}` and `{{RESOURCE_ID}}` parameters yield the instance ID only\. For more information, see [Supported pseudo parameters](#pseudo-parameters)\.

## Pseudo parameter examples<a name="pseudo-parameter-examples"></a>

Suppose that your payload for a Lambda task needs to reference an instance by its ID\.

Whether you’re using an `INSTANCE` or a `RESOURCE_GROUP` maintenance window target, this can be achieved by using the `{{RESOURCE_ID}}` pseudo parameter:

```
"TaskArn": "arn:aws:lambda:us-east-2:111122223333:function:SSMTestFunction",
    "TaskType": "LAMBDA",
    "TaskInvocationParameters": {
        "Lambda": {
            "ClientContext": "ew0KICAi--truncated--0KIEXAMPLE",
            "Payload": "{ \"instanceId\": \"{{RESOURCE_ID}}\" }",
            "Qualifier": "$LATEST"
        }
    }
```

If your Lambda task is intended to run against another supported target type in addition to EC2 instances, such as a DynamoDB table, the same syntax can be used, and `{{RESOURCE_ID}}` yields the name of the table only\. However, if you require the full ARN of the table, use `{{TARGET_ID}}`, as shown in the following example\.

```
"TaskArn": "arn:aws:lambda:us-east-2:111122223333:function:SSMTestFunction",
    "TaskType": "LAMBDA",
    "TaskInvocationParameters": {
        "Lambda": {
            "ClientContext": "ew0KICAi--truncated--0KIEXAMPLE",
            "Payload": "{ \"tableArn\": \"{{TARGET_ID}}\" }",
            "Qualifier": "$LATEST"
        }
    }
```

The same syntax works for targeting instances or other resource types\. When multiple resource types have been added to a resource group, the task runs against each of the appropriate resources\. 

**Important**  
Not all resource types that might be included in a resource group yield a value for the `{{RESOURCE_ID}}` parameter\. For a list of supported resource types, see [Supported pseudo parameters](#pseudo-parameters)\.

As another example, to run an Automation task that stops your EC2 instances, you specify the `AWS-StopEC2Instance` SSM document as the `TaskArn` value and use the `{{RESOURCE_ID}` pseudo parameter:

```
"TaskArn": "AWS-StopEC2Instance",
    "TaskType": "AUTOMATION"
    "TaskInvocationParameters": {
        "Automation": {
            "DocumentVersion": "1",
            "Parameters": {
                "instanceId": [
                    "{{RESOURCE_ID}}"
                ]
            }
        }
    }
```

To run an Automation task that copies a snapshot of an Amazon Elastic Block Store \(Amazon EBS\) volume, you specify the `AWS-CopySnapshot` SSM document as the `TaskArn` value and use the `{{RESOURCE_ID}` pseudo parameter:

```
"TaskArn": "AWS-CopySnapshot",
    "TaskType": "AUTOMATION"
    "TaskInvocationParameters": {
        "Automation": {
            "DocumentVersion": "1",
            "Parameters": {
                "SourceRegion": "us-east-2",
                "targetType":"RESOURCE_GROUP",
                "SnapshotId": [
                    "{{RESOURCE_ID}}"
                ]
            }
        }
    }
```

## Supported pseudo parameters<a name="pseudo-parameters"></a>

The following list describes the pseudo parameters that you can specify using the `{{PSEUDO_PARAMETER}}` syntax in the `--task-invocation-parameters` option\.
+ **`WINDOW_ID`**: The ID of the target maintenance window\.
+ **`WINDOW_TASK_ID`**: The ID of the window task that is executing\.
+ **`WINDOW_TARGET_ID`**: The ID of the window target that includes the target \(target ID\)\.
+ **`WINDOW_EXECUTION_ID`**: The ID of the current window execution\.
+ **`TASK_EXECUTION_ID`**: The ID of the current task execution\.
+ **`INVOCATION_ID`**: The ID of the current invocation\.
+ **`TARGET_TYPE`**: The type of target\. Supported types include `RESOURCE_GROUP` and `INSTANCE`\.
+ **`TARGET_ID`**: 

  If the target type you specify is `INSTANCE`, the `TARGET_ID` pseudo parameter is replaced by the ID of the instance; for example, `i-078a280217EXAMPLE`\.

  If the target type you specify is `RESOURCE_GROUP`, the value referenced for the task execution is the full ARN of the resource; for example: `arn:aws:ec2:us-east-1:123456789012:instance/i-078a280217EXAMPLE`\. The following table provides sample `TARGET_ID` values for particular resource types in a resource group\.     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/mw-cli-register-tasks-parameters.html)
+ **`RESOURCE_ID`**: The short ID of a resource type contained in a resource group\. The following table provides sample `RESOURCE_ID` values for particular resource types in a resource group\.     
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/mw-cli-register-tasks-parameters.html)
**Note**  
If the AWS resource group you specify includes resource types that do not yield a `RESOURCE_ID` value, and are not listed in the table above, then the `RESOURCE_ID` parameter is not populated\. An execution invocation will still occur for that resource\. In these cases, use the `TARGET_ID` pseudo parameter instead, which will be replaced with the full ARN of the resource\.