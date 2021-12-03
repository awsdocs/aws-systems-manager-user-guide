# Troubleshooting maintenance windows<a name="troubleshooting-maintenance-windows"></a>

Use the following information to help you troubleshoot problems with maintenance windows\.

**Topics**
+ [Edit task error: On the page for editing a maintenance window task, the IAM role list returns an error message: "We couldn't find the IAM maintenance window role specified for this task\. It might have been deleted, or it might not have been created yet\."](#maintenance-window-role-troubleshooting)
+ [Not all maintenance window targets are updated](#targets-not-updated)
+ [Task fails with error message: "Step fails when it is validating and resolving the step inputs"](#step-fails)
+ [Error messages: "Maintenance window tasks without targets don't support MaxConcurrency values" and "Maintenance window tasks without targets don't support MaxErrors values"](#maxconcurrency-maxerrors-not-supported)

## Edit task error: On the page for editing a maintenance window task, the IAM role list returns an error message: "We couldn't find the IAM maintenance window role specified for this task\. It might have been deleted, or it might not have been created yet\."<a name="maintenance-window-role-troubleshooting"></a>

**Problem 1**: The AWS Identity and Access Management \(IAM\) maintenance window role you originally specified was deleted after you created the task\.

**Possible fixes**: \(1\) Select a different IAM maintenance window role, if one exists in your account, or create a new one and select it for the task\. \(2\) Create or select an AWS Systems Manager service\-linked role\. For more information, see [Should I use a service\-linked role or a custom service role to run maintenance window tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)\.

**Problem 2**: If the task was created using the AWS Command Line Interface \(AWS CLI\), AWS Tools for Windows PowerShell, or an AWS SDK, a non\-existent IAM maintenance window role name could have been specified\. For example, the IAM maintenance window role could have been deleted before you created the task, or the role name could have been typed incorrectly, such as **myrole** instead of **my\-role**\.

**Possible fixes**: \(1\) Select the correct name of the IAM maintenance window role you want to use, or create a new one to specify for the task\. \(2\) Create or select a Systems Manager service\-linked role\. For more information, see [Should I use a service\-linked role or a custom service role to run maintenance window tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)\.

## Not all maintenance window targets are updated<a name="targets-not-updated"></a>

**Problem:** You notice that maintenance window tasks didn't run on all the resources targeted by your maintenance window\. For example, in the maintenance window run results, the task for that resource is marked as failed or timed out\.

**Solution:** The most common reasons for a maintenance window task not running on a target resource involve connectivity and availability\. For example:
+ Systems Manager lost connection to the resource before or during the maintenance window operation\.
+ The resource was offline or stopped during the maintenance window operation\.

You can wait for the next scheduled maintenance window time to run tasks on the resources\. You can manually run the maintenance window tasks on the resources that weren't available or were offline\.

## Task fails with error message: "Step fails when it is validating and resolving the step inputs"<a name="step-fails"></a>

**Problem**: An Automation runbook or Systems Manager Command document you're using in a task requires that you specify inputs such as `InstanceId` or `SnapshotId`, but a value isn't supplied or isn't supplied correctly\.
+ **Solution 1**: If your task is targeting a single resource, such as a single node or single snapshot, enter its ID in the input parameters for the task\.
+ **Solution 2**: If your task is targeting multiple resources, such as creating images from multiple nodes when you use the runbook `AWS-CreateImage`, you can use one of the pseudo parameters supported for maintenance window tasks in the input parameters to represent node IDs in the command\. 

  The following commands register a Systems Manager Automation task with a maintenance window using the AWS CLI\. The `--targets` value indicates a maintenance window target ID\. Also, even though the `--targets` parameter specifies a window target ID, parameters of the Automation runbook require that a node ID be provided\. In this case, the command uses the pseudo parameter `{{RESOURCE_ID}}` as the `InstanceId` value\.

  **AWS CLI command:**

------
#### [ Linux & macOS ]

  The following command restarts Amazon Elastic Compute Cloud \(Amazon EC2\) instances that belong to the maintenance window target group with the ID e32eecb2\-646c\-4f4b\-8ed1\-205fbEXAMPLE\.

  ```
  aws ssm register-task-with-maintenance-window \
      --window-id "mw-0c50858d01EXAMPLE" \
      --targets Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE \
      --task-arn "AWS-RestartEC2Instance" \
      --service-role-arn arn:aws:iam::123456789012:role/MyMaintenanceWindowServiceRole \
      --task-type AUTOMATION \
      --task-invocation-parameters "Automation={DocumentVersion=5,Parameters={InstanceId='{{RESOURCE_ID}}'}}" \
      --priority 0 --max-concurrency 10 --max-errors 5 --name "My-Restart-EC2-Instances-Automation-Task" \
      --description "Automation task to restart EC2 instances"
  ```

------
#### [ Windows ]

  ```
  aws ssm register-task-with-maintenance-window ^
      --window-id "mw-0c50858d01EXAMPLE" ^
      --targets Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE ^
      --task-arn "AWS-RestartEC2Instance" ^
      --service-role-arn arn:aws:iam::123456789012:role/MyMaintenanceWindowServiceRole ^
      --task-type AUTOMATION ^
      --task-invocation-parameters "Automation={DocumentVersion=5,Parameters={InstanceId='{{RESOURCE_ID}}'}}" ^
      --priority 0 --max-concurrency 10 --max-errors 5 --name "My-Restart-EC2-Instances-Automation-Task" ^
      --description "Automation task to restart EC2 instances"
  ```

------

  For more information about working with pseudo parameters for maintenance window tasks, see [About pseudo parameters](mw-cli-register-tasks-parameters.md) and [Task registration examples](mw-cli-register-tasks-examples.md#task-examples)\.

## Error messages: "Maintenance window tasks without targets don't support MaxConcurrency values" and "Maintenance window tasks without targets don't support MaxErrors values"<a name="maxconcurrency-maxerrors-not-supported"></a>

**Problem:** When you register a Run Command\-type task, you must specify at least one target for the task to run on\. For other task types \(Automation, AWS Lambda, and AWS Step Functions\), depending on the nature of the task, targets are optional\. The options `MaxConcurrency` \(the number of resources to run a task on at the same time\) and `MaxErrors` \(the number of failures to run the task on target resources before the task fails\) aren't required or supported for maintenance window tasks that don't specify targets\. The system generates these error messages if values are specified for either of these options when no task target is specified\.

**Solution**: If you receive either of these errors, remove the values for concurrency and error threshold before continuing to register or update the maintenance window task\.

For more information about running tasks that don't specify targets, see [Registering maintenance window tasks without targets](maintenance-windows-targetless-tasks.md) in the *AWS Systems Manager User Guide*\.