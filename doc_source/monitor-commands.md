# Understanding Command Statuses<a name="monitor-commands"></a>

Systems Manager Run Command reports detailed status information about the different states a command experiences during processing and for each instance that processed the command\. You can monitor command statuses using the following methods\.
+ Click the **Refresh** icon on the **Run Command** page in the Amazon EC2 console\.
+ Call [list\-commands](http://docs.aws.amazon.com/cli/latest/reference/ssm/list-commands.html) or [list\-command\-invocations](http://docs.aws.amazon.com/cli/latest/reference/ssm/list-command-invocations.html.html) using the AWS CLI\. Or call [Get\-SSMCommand](http://docs.aws.amazon.com/powershell/latest/reference/items/Get-SSMCommand.html) or [Get\-SSMCommandInvocation](http://docs.aws.amazon.com/powershell/latest/reference/items/Get-SSMCommandInvocation.html) using AWS Tools for Windows PowerShell\.
+ Configure CloudWatch Events to log status changes\.
+ Configure Amazon SNS to send notifications for all status changes or specific statuses like Failed or TimedOut\.

## Run Command Status<a name="monitor-about-status"></a>

Run Command reports status details for three areas: plugins, invocations, and an overall command status\. A *plugin* is a code\-execution block that is defined in your command's Systems Manager document\. The AWS\-\* documents include only one plugin, but you can create your own documents that use multiple plugins\. For more information about plugins, see [SSM Document Plugin Reference](ssm-plugins.md)\.

When you send a command to multiple instances at the same time, each copy of the command targeting each instance is a *command invocation*\. For example, if you use the AWS\-RunShellScript document and send an ifconfig command to 20 instances, that command has 20 invocations\. Each command invocation individually reports status\. The plugins for a given command invocation individually report status as well\. 

Lastly, Run Command includes an aggregated command status for all plugins and invocations\. The aggregated command status can be different than the status reported by plugins or invocations, as noted in the following tables\.

**Note**  
If you run commands to large numbers of instances using the `max-concurrency` or `max-errors` parameters, command status reflects the limits imposed by those parameters, as described in the following tables\. For more information about these parameters, see [Sending Commands to a Fleet](send-commands-multiple.md)\.


**Detailed Status for Command Plugins and Invocations**  

| Status | Details | 
| --- | --- | 
| Pending | The command was not yet received by the agent on the instance\. If the command is not received by the agent before the value specified by the Timeout \(seconds\) parameter is reached, then the status changes to Delivery Timed Out\. | 
| In Progress | The command was received by the agent, or the command started executing on the instance\. Depending on the result of all command plugins, the status will change to Success, Failed, or Execution Timed Out\. If the agent is not available on the instance, the command status will show In Progress until the agent is available again\. The status will then change to a terminal state\.  | 
| Delayed | The system attempted to send the command to the instance but was not successful\. The system will retry again\. | 
| Success | The command was received by SSM Agent on the instance and returned an exit code of zero\. This status does not mean the command was successfully processed on the instance\. This is a terminal state\.  To troubleshoot errors or get more information about the command execution, send a command that handles errors or exceptions by returning appropriate exit codes \(non\-zero exit codes for command failure\)\.  | 
| Delivery Timed Out | The command was not delivered to the instance before the delivery timeout expired\. Delivery timeouts do not count against the parent command’s max\-errors limit, but they do contribute to whether the parent command status is Success or Incomplete\. This is a terminal state\. | 
| Execution Timed Out | Command execution started on the instance, but the execution was not complete before the execution timeout expired\. Execution timeouts count against the max\-errors limit of the parent command\. This is a terminal state\. | 
| Failed |  The command was not successful on the instance\. For a plugin, this indicates that the result code was not zero\. For a command invocation, this indicates that the result code for one or more plugins was not zero\. Invocation failures count against the `max-errors` limit of the parent command\. This is a terminal state\.  | 
| Canceled | The command was terminated before it was completed\. This is a terminal state\. | 
| Undeliverable | The command can't be delivered to the instance\. The instance might not exist or it might not be responding\. Undeliverable invocations don't count against the parent command’s max\-errors limit, and they don't contribute to whether the parent command status is Success or Incomplete\. This is a terminal state\. | 
| Terminated | The parent command exceeded its max\-errors limit and subsequent command invocations were canceled by the system\. This is a terminal state\. | 


**Detailed Status for a Command**  

| Status | Details | 
| --- | --- | 
| Pending | The command was not yet received by an agent on any instances\. | 
| In Progress | The command has been sent to at least one instance but has not reached a final state on all instances\.  | 
| Delayed | The system attempted to send the command to the instance but was not successful\. The system will retry again\. | 
| Success | The command was received by SSM Agent on all specified or targeted instances and returned an exit code of zero\. All command invocations have reached a terminal state, and the value of max\-errors was not reached\. This status does not mean the command was successfully processed on all specified or targeted instances\. This is a terminal state\.  To troubleshoot errors or get more information about the command execution, send a command that handles errors or exceptions by returning appropriate exit codes \(non\-zero exit codes for command failure\)\.  | 
| Delivery Timed Out | The command was not delivered to the instance before the delivery timeout expired\. The value of max\-errors or more command invocations shows a status of Delivery Timed Out\. This is a terminal state\. | 
| Execution Timed Out | Command execution started on the instance, but the execution was not complete before the execution timeout expired\. The value of max\-errors or more command invocations shows a status of Execution Timed Out\. This is a terminal state\. | 
| Failed |  The command was not successful on the instance\. The value of `max-errors` or more command invocations shows a status of `Failed`\. This is a terminal state\.  | 
| Incomplete | The command was attempted on all instances and one or more of the invocations does not have a value of Success\. However, not enough invocations failed for the status to be Failed\. This is a terminal state\. | 
| Canceled | The command was terminated before it was completed\. This is a terminal state\. | 
| Rate Exceeded | The number of instances targeted by the command exceeded the account limit for pending invocations\. The system has canceled the command before executing it on any instance\. This is a terminal state\. | 