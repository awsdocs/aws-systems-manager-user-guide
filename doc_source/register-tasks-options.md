# About 'register\-task\-with\-maintenance\-window' Options and Values<a name="register-tasks-options"></a>

Refer to the following sections to learn about the values and parameters you can supply when using the [register\-task\-with\-maintenance\-window](https://docs.aws.amazon.com/cli/latest/reference/ssm/register-task-with-maintenance-window.html) command\.

## Required Values for the 'register\-task\-with\-maintenance\-window' Command<a name="register-tasks-options-required"></a>

When you run the [register\-task\-with\-maintenance\-window](https://docs.aws.amazon.com/cli/latest/reference/ssm/register-task-with-maintenance-window.html) command, you must provide values for the following options:

**\-\-window\-id**  
The ID of the Maintenance Window to register the task with\. For example: `mw-0c5ed765acEXAMPLE`\.

**\-\-targets**  
 The instances, or Maintenance Window targets, for the task to run on\.
+ To specify a target that is already registered with a Maintenance Window, use its window target ID\. For example: `Key=WindowTargetIds,Values=0fa20143-009f-421f-941f-2b86cEXAMPLE`\.
+ To specify an instance whether it's registered as a Maintenance Window target or not, use its instance ID\. For example: `Key=InstanceIds,Values=i-1234567890EXAMPLE`\.

**\-\-task\-arn**  
The resource that the task uses during execution\.
+ For RUN\_COMMAND and AUTOMATION task types, `TaskArn` is the SSM document name or ARN\. For example: `AWS-RunBatchShellScript` or `arn:aws:ssm:us-east-2:111122223333:document/My-Document`\.
+ For LAMBDA tasks, it's the function name or ARN\. For example: `My-Lambda-Function` or `arn:aws:lambda:us-east-2:111122223333:function:My-Lambda-Function`\.
+ For STEP\_FUNCTION tasks, it's the state machine ARN\. For example: `arn:aws:states:us-east-2:111122223333:stateMachine:My-StateMachine`\.

**\-\-task\-type**  
The type of task\. The type value can be one of the following: `RUN_COMMAND`, `AUTOMATION`, `LAMBDA`, or `STEP_FUNCTION`\.

**\-\-max\-concurrency**  
The maximum number of instances that are allowed to run the command at the same time\. You can specify a number such as `10` or a percentage such as `10%`\.

**\-\-max\-errors**  
The maximum number of errors allowed without the command failing\. When the command fails one more time beyond the value of `MaxErrors`, the system stops sending the command to additional targets\. You can specify a number such as `10` or a percentage such as `10%`\.

**\-\-task\-invocation\-parameters**  
The parameters that are unique to the type of task you specified\. 

The following list describes some of the common parameters that you can specify when using `task-invocation-parameters`\. You specify these parameters by using the `{{ PARAMETER_NAME }}` syntax, as demonstrated in examples in [Create a Maintenance Window Task](register-tasks-tutorial.md)\.
+ **`TARGET_ID`**: The ID of the target\. If the target type is INSTANCE \(currently the only supported type\), then the target ID is the instance ID\.
+ **`TARGET_TYPE`**: The type of target\. Currently only INSTANCE is supported\.
+ **`WINDOW_ID`**: The ID of the target Maintenance Window\.
+ **`WINDOW_TASK_ID`**: The ID of the window task that is executing\.
+ **`WINDOW_TARGET_ID`**: The ID of the window target that includes the target \(target ID\)\.
+ **`WINDOW_EXECUTION_ID`**: The ID of the current window execution\.
+ **`TASK_EXECUTION_ID`**: The ID of the current task execution\.
+ **`INVOCATION_ID`**: The ID of the current invocation\.

## Optional Values for the 'register\-task\-with\-maintenance\-window' Command<a name="register-tasks-options-optional"></a>

In addition to the required options for the [register\-task\-with\-maintenance\-window](https://docs.aws.amazon.com/cli/latest/reference/ssm/register-task-with-maintenance-window.html) command, you can choose to provide values for these additional options:

**\-\-name**  
A name to help identify the task\.

**\-\-description**  
A description to help identify the purpose of the task\.

**\-\-service\-role\-arn**  
The role to assume when running the Maintenance Window task\.

If you do not specify a service role ARN, Systems Manager will create or use your account's service\-linked role for Systems Manager by default\. For more information, see [Should I Use a Service\-Linked Role or a Custom Service Role to Run Maintenance Window Tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)\. 

**\-\-priority**  
The priority of the task in the Maintenance Window\. The lower the number the higher the priority \(for example, 1 is highest priority\)\. Tasks in a Maintenance Window are scheduled in priority order\. Tasks that have the same priority are scheduled in parallel\.

**\-\-client\-token**  
An idempotency token that you provide\.