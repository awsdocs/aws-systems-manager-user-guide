# Understanding automation statuses<a name="automation-statuses"></a>

AWS Systems Manager Automation reports detailed status information about the various statuses an automation action or step goes through when you run an automation and for the overall automation\. Automation is a capability of AWS Systems Manager\. You can monitor automation statuses using the following methods:
+ Monitor the **Execution status** in the Systems Manager Automation console\.
+ Use your preferred command line tools\. For the AWS Command Line Interface \(AWS CLI\), you can use [describe\-automation\-step\-executions](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-automation-step-executions.html) or [get\-automation\-execution](https://docs.aws.amazon.com/cli/latest/reference/ssm/get-automation-execution.html)\. For the AWS Tools for Windows PowerShell, you can use [Get\-SSMAutomationStepExecution](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-SSMAutomationStepExecution.html) or [Get\-SSMAutomationExecution](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-SSMAutomationExecution.html)\.
+ Configure Amazon EventBridge to respond to action or automation status changes\.

## About automation statuses<a name="automation-statuses-about"></a>

Automation reports status details for individual automation actions as well as the overall automation\.

The overall automation status can be different than the status reported by an individual action or step as noted in the following tables\.


**Detailed status for actions**  

| Status | Details | 
| --- | --- | 
| Pending | The step has not started running\. If your automation uses conditional actions, steps remain in this state after an automation has completed if the condition was not met to run the step\. Steps also remain in this state if the automation is canceled before the step runs\. | 
| InProgress | The step is running\. | 
| Waiting | The step is waiting for input\. | 
| Success | The step completed successfully\. This is a terminal state\. | 
| TimedOut | A step or approval was not completed before the specified timeout period\. This is a terminal state\. | 
| Cancelling | The step is in the process of stopping after being canceled by a requester\. | 
| Cancelled | The step was stopped by a requester before it completed\. This is a terminal state\. | 
| Failed |  The step did not complete successfully\. This is a terminal state\.  | 


**Detailed status for an automation**  

| Status | Details | 
| --- | --- | 
| Pending | The automation has not started running\. | 
| InProgress | The automation is running\. | 
| Waiting | The automation is waiting for input\. | 
| Success | The automation completed successfully\. This is a terminal state\. | 
| TimedOut | A step or approval was not completed before the specified timeout period\. This is a terminal state\. | 
| Cancelling | The automation is in the process of stopping after being canceled by a requester\. | 
| Cancelled | The automation was stopped by a requester before it completed\. This is a terminal state\. | 
| Failed |  The automation did not complete successfully\. This is a terminal state\.  | 