# About register\-task\-with\-maintenance\-windows options<a name="mw-cli-task-options"></a>

The register\-task\-with\-maintenance\-window command provides several options for configuring a task according to your needs\. Some are required, some are optional, and some apply to only a single maintenance window task type\.

This topic provides information about some of these options to help you work with samples in this tutorial section\. For information about all command options, see [register\-task\-with\-maintenance\-window](https://docs.aws.amazon.com/cli/latest/reference/ssm/register-task-with-maintenance-window.html) in the *AWS CLI Command Reference*\.

**About the `--task-arn` option**  
The option `--task-arn` is used to specify the resource that the task uses during execution\. The value that you specify depends on the type of task you are registering, as described in the following table\.


**TaskArn formats for maintenance window tasks**  

| Maintenance window task type | TaskArn value | 
| --- | --- | 
|  **`RUN_COMMAND`** and ** `AUTOMATION`**  |  `TaskArn` is the SSM document name or ARN\. For example:  `AWS-RunBatchShellScript`  \-or\- `arn:aws:ssm:us-east-2:111122223333:document/My-Document`\.  | 
|  **`LAMBDA`**  |  `TaskArn` is the function name or ARN\. For example:  `SSMMy-Lambda-Function` \-or\- `arn:aws:lambda:us-east-2:111122223333:function:SSMMyLambdaFunction`\.  The IAM policy for maintenance windows requires that you prefix Lambda function \(or alias\) names with `SSM`\. Before you register this type of task, you must update its name in AWS Lambda to include `SSM`\. For example, if your Lambda function name is `MyLambdaFunction`, change it to `SSMMyLambdaFunction`\.   | 
|  **`STEP_FUNCTIONS`**  |  `TaskArn` is the state machine ARN\. For example:  `arn:aws:states:us-east-2:111122223333``:stateMachine:SSMMyStateMachine`\.  The IAM policy for maintenance windows requires that you prefix Step Functions state machine names with `SSM`\. Before you register this type of task, you must update its name in AWS Step Functions to include `SSM`\. For example, if your state machine name is `MyStateMachine`, change it to `SSMMyStateMachine`\.   | 

**About the `--service-role-arn` option**  
The role for Systems Manager to assume when running the maintenance window task\.

Specifying a service role ARN is optional\. If you do not specify a service role ARN, Systems Manager creates a service\-linked role or uses your account's service\-linked role\. 

Note that the service\-linked role for Systems Manager doesn't provide the permissions needed for all scenarios\. For more information, see [Should I use a service\-linked role or a custom service role to run maintenance window tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)

**About the `--task-invocation-parameters` option**  
The `--task-invocation-parameters` option is used to specify the parameters that are unique to each of the four task types\. The supported parameters for each of the four task types are described in the following table\.

**Note**  
For information about using pseudo parameters in `--task-invocation-parameters` content, such as \{\{TARGET\_ID\}\}, see [About pseudo parameters](mw-cli-register-tasks-parameters.md)\. 

Task invocation parameters options for maintenance window tasks


| Maintenance window task type | Available parameters  | Example | 
| --- | --- | --- | 
|  RUN\_COMMAND  |  Comment  DocumentHash  DocumentHashType  NotificationConfig  OutputS3BucketName  OutPutS3KeyPrefix  Parameters  ServiceRoleArn  TimeoutSeconds  |  <pre>"TaskInvocationParameters": {<br />        "RunCommand": {<br />            "Comment": "My Run Command task comment",<br />            "DocumentHash": "6554ed3d--truncated--5EXAMPLE",<br />            "DocumentHashType": "Sha256",<br />            "NotificationConfig": {<br />                "NotificationArn": "arn:aws:sns:us-east-2:123456789012:my-sns-topic-name",<br />                "NotificationEvents": [<br />                    "FAILURE"<br />                ],<br />                "NotificationType": "Invocation"<br />            },<br />            "OutputS3BucketName": "DOC-EXAMPLE-BUCKET",<br />            "OutputS3KeyPrefix": "DOC-EXAMPLE-FOLDER",<br />            "Parameters": {<br />                "commands": [<br />                    "Get-ChildItem$env: temp-Recurse|Remove-Item-Recurse-force"<br />                ]<br />            },<br />            "ServiceRoleArn": "arn:aws:iam::123456789012:role/MyMaintenanceWindowServiceRole",<br />            "TimeoutSeconds": 3600<br />        }<br />    }</pre>  | 
|  AUTOMATION  |  DocumentVersion Parameters  |  <pre>"TaskInvocationParameters": {<br />        "Automation": {<br />            "DocumentVersion": "3",<br />            "Parameters": {<br />                "instanceid": [<br />                    "{{TARGET_ID}}"<br />                ]<br />            }<br />        }<br />    }</pre>  | 
|  LAMBDA  |  ClientContext Payload Qualifier  |  <pre>"TaskInvocationParameters": {<br />        "Lambda": {<br />            "ClientContext": "ew0KICAi--truncated--0KIEXAMPLE",<br />            "Payload": "{ \"targetId\": \"{{TARGET_ID}}\", \"targetType\": \"{{TARGET_TYPE}}\" }",<br />            "Qualifier": "$LATEST"<br />        }<br />    }</pre>  | 
|  STEP\_FUNCTIONS  |  Input Name  |  <pre>"TaskInvocationParameters": {<br />        "StepFunctions": {<br />            "Input": "{ \"targetId\": \"{{TARGET_ID}}\" }",<br />            "Name": "{{INVOCATION_ID}}"<br />        }<br />    }</pre>  | 