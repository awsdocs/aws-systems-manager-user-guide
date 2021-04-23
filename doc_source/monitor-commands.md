# Understanding command statuses<a name="monitor-commands"></a>

Systems Manager Run Command reports detailed status information about the different states a command experiences during processing and for each instance that processed the command\. You can monitor command statuses using the following methods\.
+ Click the **Refresh** icon on the **Run Command** page in the Amazon EC2 console\.
+ Call [list\-commands](https://docs.aws.amazon.com/cli/latest/reference/ssm/list-commands.html) or [list\-command\-invocations](https://docs.aws.amazon.com/cli/latest/reference/ssm/list-command-invocations.html.html) using the AWS CLI\. Or call [Get\-SSMCommand](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-SSMCommand.html) or [Get\-SSMCommandInvocation](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-SSMCommandInvocation.html) using AWS Tools for Windows PowerShell\.
+ Configure EventBridge to respond to state or status changes\.
+ Configure Amazon SNS to send notifications for all status changes or specific statuses like Failed or TimedOut\.

## Run Command status<a name="monitor-about-status"></a>

Run Command reports status details for three areas: plugins, invocations, and an overall command status\. A *plugin* is a code\-execution block that is defined in your command's Systems Manager \(SSM\) document\. For more information about plugins, see [Systems Manager Command document plugin reference](ssm-plugins.md)\.

When you send a command to multiple instances at the same time, each copy of the command targeting each instance is a *command invocation*\. For example, if you use the `AWS-RunShellScript` document and send an `ifconfig` command to 20 Linux instances, that command has 20 invocations\. Each command invocation individually reports status\. The plugins for a given command invocation individually report status as well\. 

Lastly, Run Command includes an aggregated command status for all plugins and invocations\. The aggregated command status can be different than the status reported by plugins or invocations, as noted in the following tables\.

**Note**  
If you run commands to large numbers of instances using the `max-concurrency` or `max-errors` parameters, command status reflects the limits imposed by those parameters, as described in the following tables\. For more information about these parameters, see [Using targets and rate controls to send commands to a fleet](send-commands-multiple.md)\.


**Detailed status for command plugins and invocations**  

| Status | Details | 
| --- | --- | 
| Pending | The command has not yet been sent to the instance or has not been received by SSM Agent\. If the command isn't received by the agent before the length of time passes that is equal to the sum of the Timeout \(seconds\) parameter and the Execution timeout parameter, the status eventually changes to Delivery Timed Out\. | 
| In Progress | Systems Manager is attempting to send the command to the instance, or the command was received by SSM Agent and has started running on the instance\. Depending on the result of all command plugins, the status changes to Success, Failed, Delivery Timed Out, or Execution Timed Out\. Exception: If the agent isn't running or available on the instance, the command status remains at In Progress until the agent is available again, or until the execution timeout limit is reached\. The status then changes to a terminal state\. | 
| Delayed | The system attempted to send the command to the instance but wasn't successful\. The system retries again\. | 
| Success | The command was received by SSM Agent on the instance and returned an exit code of zero\. This status doesn't mean the command was successfully processed on the instance\. This is a terminal state\.  To troubleshoot errors or get more information about the command execution, send a command that handles errors or exceptions by returning appropriate exit codes \(non\-zero exit codes for command failure\)\.  | 
| Delivery Timed Out | The command wasn't delivered to the instance before the delivery timeout expired\. Delivery timeouts don't count against the parent command’s max\-errors limit, but they do contribute to whether the parent command status is Success or Incomplete\. This is a terminal state\. | 
| Execution Timed Out | Command execution started on the instance, but the execution wasn't complete before the execution timeout expired\. Execution timeouts count against the max\-errors limit of the parent command\. This is a terminal state\. When the timeout is reached, Systems Manager stops the command execution\. | 
| Failed |  The command wasn't successful on the instance\. For a plugin, this indicates that the result code wasn't zero\. For a command invocation, this indicates that the result code for one or more plugins wasn't zero\. Invocation failures count against the `max-errors` limit of the parent command\. This is a terminal state\.  | 
| Canceled | The command was terminated before it was completed\. This is a terminal state\. | 
| Undeliverable | The command can't be delivered to the instance\. The instance might not exist or it might not be responding\. Undeliverable invocations don't count against the parent command’s max\-errors limit, but they do contribute to whether the parent command status is Success or Incomplete\. For example, if all invocations in a command have the status Undeliverable, then the command status returned is Failed\. However, if a command has five invocations, four of which return the status Undeliverable and one of which returns the status Success, then the parent command's status is Success\. This is a terminal state\. | 
| Terminated | The parent command exceeded its max\-errors limit and subsequent command invocations were canceled by the system\. This is a terminal state\. | 
| Invalid Platform | The command was sent to an instance that did not match the required platforms specified by the chosen document\. Invalid Platform doesn't count against the parent command’s max\-errors limit, but it does contribute to whether the parent command status is Success or Failed\. For example, if all invocations in a command have the status Invalid Platform, then the command status returned is Failed\. However, if a command has five invocations, four of which return the status Invalid Platform and one of which returns the status Success, then the parent command's status is Success\. This is a terminal state\. | 
| Access Denied | The IAM user or role initiating the command doesn't have access to the targeted managed instance\. Access Denied doesn't count against the parent command’s max\-errors limit, but it does contribute to whether the parent command status is Success or Failed\. For example, if all invocations in a command have the status Access Denied, then the command status returned is Failed\. However, if a command has five invocations, four of which return the status Access Denied and one of which returns the status Success, then the parent command's status is Success\. This is a terminal state\. | 


**Detailed status for a command**  

| Status | Details | 
| --- | --- | 
| Pending | The command wasn't yet received by an agent on any instances\. | 
| In Progress | The command has been sent to at least one instance but hasn't reached a final state on all instances\.  | 
| Delayed | The system attempted to send the command to the instance but wasn't successful\. The system retries again\. | 
| Success | The command was received by SSM Agent on all specified or targeted instances and returned an exit code of zero\. All command invocations have reached a terminal state, and the value of max\-errors wasn't reached\. This status doesn't mean the command was successfully processed on all specified or targeted instances\. This is a terminal state\.  To troubleshoot errors or get more information about the command execution, send a command that handles errors or exceptions by returning appropriate exit codes \(non\-zero exit codes for command failure\)\.  | 
| Delivery Timed Out | The command wasn't delivered to the instance before the delivery timeout expired\. The value of max\-errors or more command invocations shows a status of Delivery Timed Out\. This is a terminal state\. | 
| Execution Timed Out | Command execution started on the instance, but the execution wasn't complete before the execution timeout expired\. The value of max\-errors or more command invocations shows a status of Execution Timed Out\. This is a terminal state\. | 
| Failed |  The command wasn't successful on the instance\. The value of `max-errors` or more command invocations shows a status of `Failed`\. This is a terminal state\.  | 
| Incomplete | The command was attempted on all instances and one or more of the invocations doesn't have a value of Success\. However, not enough invocations failed for the status to be Failed\. This is a terminal state\. | 
| Canceled | The command was terminated before it was completed\. This is a terminal state\. | 
| Rate Exceeded | The number of instances targeted by the command exceeded the account limit for pending invocations\. The system has canceled the command before executing it on any instance\. This is a terminal state\. | 
| Access Denied | The IAM user or role initiating the command doesn't have access to the targeted resource group\. AccessDenied doesn't count against the parent command’s max\-errors limit, but does contribute to whether the parent command status is Success or Failed\. \(For example, if all invocations in a command have the status AccessDenied, then the command status returned is Failed\. However, if a command has 5 invocations, 4 of which return the status AccessDenied and 1 of which returns the status Success, then the parent command's status is Success\.\) This is a terminal state\. | 
| No Instances In Tag | The tag key\-pair value or resource group targeted by the command doesn't match any managed instances\. This is a terminal state\. | 

## Understanding command timeout values<a name="monitor-about-status-timeouts"></a>

Systems Manager enforces the following timeout values when running commands\.

**Delivery Timeout**  
In the Systems Manager console, you specify the delivery timeout value in the **Timeout \(seconds\)** field\. After a command is sent, Run Command checks whether the command has expired or not\. If a command reaches the command expiration limit \(total timeout\), it changes status to `DeliveryTimedOut` for all invocations that are currently have status of `InProgress`, `Pending` or `Delayed`\.

![\[The Timeout (seconds) field in the Systems Manager console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/run-command-delivery-time-out-time-out-seconds.png)

On a more technical level, delivery timeout \(**Timeout \(seconds\)**\) is a combination of two timeout values, as shown here: 

`Delivery timeout = "Timeout(seconds)" from the console + "timeoutSeconds": "{{ executionTimeout }}" from your SSM document`

For example, the default value of **Timeout \(seconds\)** in the Systems Manager console is 600 seconds\. If you run a command by using the `AWS-RunShellScript` SSM document, the default value of **"timeoutSeconds": "\{\{ executionTimeout \}\}"** is 3600 seconds, as shown in the following document sample:

```
  "executionTimeout": {
      "type": "String",
      "default": "3600",

  "runtimeConfig": {
    "aws:runShellScript": {
      "properties": [
        {
          "timeoutSeconds": "{{ executionTimeout }}"
```

This means the command runs for 4,200 seconds \(70 minutes\) before the system returns a delivery timeout\.

**Execution Timeout**  
In the Systems Manager console, you specify the execution timeout value in the **Execution Timeout** field, if available\. Not all SSM documents require that you specify an execution timeout\. If specified, the command must complete within this time period\.

**Note**  
 Run Command relies on the SSM Agent document terminal response to determine whether or not the command was delivered to the agent\. SSM Agent must send an `ExecutionTimedOut` signal for an invocation or command to be marked as `ExecutionTimedOut`\.

![\[The Execution Timeout field in the Systems Manager console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/run-command-execution-timeout-console.png)

**Default Execution Timeout**  
If a Systems Manager \(SSM\) document doesn't require that you explicitly specify an execution timeout value, then Systems Manager enforces the hard\-coded default execution timeout\.

**Total Timeout**  
Total timeout is equal to the value of `delivery timeout` plus `execution timeout`\. If `execution timeout` isn't required by the SSM document, then `total timeout` is equal to the value of `delivery timeout` plus `default execution timeout`\.

**How Systems Manager Reports Timeouts**  
If Systems Manager receives an `execution timeout` reply from SSM Agent on a target, then Systems Manager marks the command invocation as `executionTimeout`\.

If Run Command doesn't receive a document terminal response from SSM Agent, the command invocation is marked as `deliveryTimeout`\.

To determine timeout status on a target, SSM Agent combines all of the parameters and the content of the SSM document to calculate for `executionTimeout`\. When SSM Agent determines that a command has timed out, it sends `executionTimeout` to the service\.

The default for **Timeout \(seconds\)** is 3600 seconds\. The default for **Execution Timeout** is also 3600 seconds\. Therefore, the total default timeout for a command is 7200 seconds\.

**Note**  
SSM Agent processes `executionTimeout` differently depending on the type of SSM document and the document version\. 