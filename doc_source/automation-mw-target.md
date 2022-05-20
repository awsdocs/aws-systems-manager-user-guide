# Running automations with triggers using a maintenance window<a name="automation-mw-target"></a>

You can start an automation by configuring a runbook as a registered task for a maintenance window\. By registering the runbook as a registered task, the maintenance window runs the automation during the scheduled maintenance period\. 

For example, let's say you create a runbook named `CreateAMI` that creates an Amazon Machine Image \(AMI\) of instances registered as targets to the maintenance window\. To specify the `CreateAMI` runbook \(and corresponding automation\) as a registered task of a maintenance window, you first create a maintenance window and register targets\. Then you use the following procedure to specify the `CreateAMI` document as a registered task within the maintenance window\. When the maintenance window starts during the scheduled period, the system runs the automation and creates an AMI of the registered targets\.

For information about creating Automation runbooks, see [Working with runbooks](automation-documents.md)\. Automation is a capability of AWS Systems Manager\.

Use the following procedures to configure an automation as a registered task for a maintenance window using the AWS Systems Manager console, AWS Command Line Interface \(AWS CLI\), or AWS Tools for Windows PowerShell\.

## Registering an automation task to a maintenance window \(console\)<a name="automation-mw-target-console"></a>

The following procedure describes how to use the Systems Manager console to configure an automation as a registered task for a maintenance window\.

**Before you begin**  
Before you complete the following procedure, you must create a maintenance window and register at least one target\. For more information, see the following procedures: 
+ [Create a maintenance window \(console\)](sysman-maintenance-create-mw.md)\.
+ [Assign targets to a maintenance window \(console\)](sysman-maintenance-assign-targets.md)

**To configure an automation as a registered task for a maintenance window**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the left navigation pane, choose **Maintenance Windows**, and then choose the maintenance window you want to register an Automation task with\.

1. Choose **Actions**\. Then choose **Register Automation task** to run your choice of an automation on targets by using a runbook\.

1. For **Name**, enter a name for the task\.

1. For **Description**, enter a description\.

1. For **Document**, choose the runbook that defines the tasks to run\.

1. For **Document version**, choose the runbook version to use\.

1. For **Task priority**, specify a priority for this task\. `1` is the highest priority\. Tasks in a maintenance window are scheduled in priority order; tasks that have the same priority are scheduled in parallel\.

1. In the **Targets** section, if the runbook you chose is one that runs tasks on resources, identify the targets on which you want to run this automation by specifying tags or by selecting instances manually\.

   
**Note**  
If you want to pass the resources through input parameters instead of targets, you don't need to specify a maintenance window target\.  
In many cases, you don't need to explicitly specify a target for an automation task\. For example, say that you're creating an Automation\-type task to update an Amazon Machine Image \(AMI\) for Linux using the `AWS-UpdateLinuxAmi` runbook\. When the task runs, the AMI is updated with the latest available Linux distribution packages and Amazon software\. New instances created from the AMI already have these updates installed\. Because the ID of the AMI to be updated is specified in the input parameters for the runbook, there is no need to specify a target again in the maintenance window task\.

   For information about maintenance window tasks that don't require targets, see [Registering maintenance window tasks without targets](maintenance-windows-targetless-tasks.md)\.

1. \(Optional\) For **Rate control**:
**Note**  
If the task you're running doesn't specify targets, you don;t need to specify rate controls\.
   + For **Concurrency**, specify either a number or a percentage of targets on which to run the automation at the same time\.

     If you selected targets by choosing tag key\-value pairs, and you aren't certain how many targets use the selected tags, then limit the number of automations that can run at the same time by specifying a percentage\.

     When the maintenance window runs, a new automation is initiated per target\. There is a limit of 100 concurrent automations per AWS account\. If you specify a concurrency rate greater than 100, concurrent automations greater than 100 are automatically added to the automation queue\. For information, see [Systems Manager service quotas](https://docs.aws.amazon.com/general/latest/gr/ssm.html#limits_ssm) in the *Amazon Web Services General Reference*\. 
   + For **Error threshold**, specify when to stop running the automation on other targets after it fails on either a number or a percentage of targets\. For example, if you specify three errors, then Systems Manager stops running automations when the fourth error is received\. Targets still processing the automation might also send errors\.

1. In the ** IAM service role** area, choose one of the following options to provide permissions for Systems Manager to start the automation:
   +  ** Create and use a service\-linked role for Systems Manager **

     Service\-linked roles provide a secure way to delegate permissions to AWS services because only the linked service can assume a service\-linked role\. Additionally, AWS automatically defines and sets the permissions of service\-linked roles, depending on the actions that the linked service performs on your behalf\.
**Note**  
If a service\-linked role has already been created for your account, choose **Use the service\-linked role for Systems Manager**\.
   + **Use a custom service role**

     If you want to use stricter permissions than those provided by the service\-linked role, you can create a custom service role for maintenance window tasks\.

     To create a custom service role, see one of the following topics:
     + [Control access to maintenance windows \(console\)](sysman-maintenance-perm-console.md)
     + [Control access to maintenance windows \(AWS CLI\)](sysman-maintenance-perm-cli.md)
     + [Control access to maintenance windows \(Tools for Windows PowerShell\)](sysman-maintenance-perm-ps.md)

   To help you decide whether to use a custom service role or the Systems Manager service\-linked role with a maintenance window task, see [Should I use a service\-linked role or a custom service role to run maintenance window tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)\.

1. In the **Input Parameters** section, specify parameters for the runbook\. For runbooks, the system auto\-populates some of the values\. You can keep or replace these values\.
**Important**  
For runbooks, you can optionally specify an Automation Assume Role\. If you don't specify a role for this parameter, then the automation assumes the maintenance window service role you choose in step 11\. As such, you must ensure that the maintenance window service role you choose has the appropriate AWS Identity and Access Management \(IAM\) permissions to perform the actions defined within the runbook\.   
For example, the service\-linked role for Systems Manager doesn't have the IAM permission `ec2:CreateSnapshot`, which is required to use the runbook `AWS-CopySnapshot`\. In this scenario, you must either use a custom maintenance window service role or specify an Automation Assume Role that has `ec2:CreateSnapshot` permissions\. For information, see [Setting up Automation](automation-setup.md)\.

1. Choose **Register Automation task**\.

## Registering an Automation task to a maintenance window \(command line\)<a name="automation-mw-target-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to configure an automation as a registered task for a maintenance window\.

**Before you begin**  
Before you complete the following procedure, you must create a maintenance window and register at least one target\. For more information, see the following procedures:
+ [Step 1: Create the maintenance window \(AWS CLI\)](mw-cli-tutorial-create-mw.md)\.
+ [Step 2: Register a target node with the maintenance window \(AWS CLI\)](mw-cli-tutorial-targets.md)

**To configure an automation as a registered task for a maintenance window**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you haven't already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Create a command to configure an automation as a registered task for a maintenance window\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm register-task-with-maintenance-window \
       --window-id window ID \
       --name task name \
       --task-arn runbook name \
       --targets Key=targets,Values=value \
       --service-role-arn IAM role arn \
       --task-type AUTOMATION \
       --task-invocation-parameters task parameters \
       --priority task priority \
       --max-concurrency 10% \
       --max-errors 5
   ```

**Note**  
If you configure an automation as a registered task by using the AWS CLI, use the `--Task-Invocation-Parameters` parameter to specify parameters to pass to a task when it runs\. Don't use the `--Task-Parameters` parameter\. The `--Task-Parameters` parameter is a legacy parameter\.  
For maintenance window tasks without a target specified, you can't supply values for `--max-errors` and `--max-concurrency`\. Instead, the system inserts a placeholder value of `1`, which might be reported in the response to commands such as [describe\-maintenance\-window\-tasks](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-maintenance-window-tasks.html) and [get\-maintenance\-window\-task](https://docs.aws.amazon.com/cli/latest/reference/ssm/get-maintenance-window-task.html)\. These values don't affect the running of your task and can be ignored\.  
For information about maintenance window tasks that don't require targets, see [Registering maintenance window tasks without targets](maintenance-windows-targetless-tasks.md)\.

------
#### [ Windows ]

   ```
   aws ssm register-task-with-maintenance-window ^
       --window-id window ID ^
       --name task name ^
       --task-arn runbook name ^
       --targets Key=targets,Values=value ^
       --service-role-arn IAM role arn ^
       --task-type AUTOMATION ^
       --task-invocation-parameters task parameters ^
       --priority task priority ^
       --max-concurrency 10% ^
       --max-errors 5
   ```

**Note**  
If you configure an automation as a registered task by using the AWS CLI, use the `--task-invocation-parameters` parameter to specify parameters to pass to a task when it runs\. Don't use the `--task-parameters` parameter\. The `--task-parameters` parameter is a legacy parameter\.  
For maintenance window tasks without a target specified, you can't supply values for `--max-errors` and `--max-concurrency`\. Instead, the system inserts a placeholder value of `1`, which might be reported in the response to commands such as [describe\-maintenance\-window\-tasks](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-maintenance-window-tasks.html) and [get\-maintenance\-window\-task](https://docs.aws.amazon.com/cli/latest/reference/ssm/get-maintenance-window-task.html)\. These values don't affect the running of your task and can be ignored\.  
For information about maintenance window tasks that don't require targets, see [Registering maintenance window tasks without targets](maintenance-windows-targetless-tasks.md)\.

------
#### [ PowerShell ]

   ```
   Register-SSMTaskWithMaintenanceWindow `
       -WindowId window ID `
       -Name "task name" `
       -TaskArn "runbook name" `
       -Target @{ Key="targets";Values="value" } `
       -ServiceRoleArn "IAM role arn" `
       -TaskType "AUTOMATION" `
       -Automation_Parameter @{ "task parameter"="task parameter value"} `
       -Priority task priority `
       -MaxConcurrency 10% `
       -MaxError 5
   ```

**Note**  
If you configure an automation as a registered task by using the AWS Tools for PowerShell, use the `-Automation_Parameter` parameter to specify parameters to pass to a task when the task runs\. Don't use the `-TaskParameters` parameter\. The `-TaskParameters` parameter is a legacy parameter\.  
For maintenance window tasks without a target specified, you can't supply values for `-MaxError` and `-MaxConcurrency`\. Instead, the system inserts a placeholder value of 1, which might be reported in the response to commands such as `Get-SSMMaintenanceWindowTaskList` and `Get-SSMMaintenanceWindowTask`\. These values don't affect the running of your task and can be ignored\.  
For information about maintenance window tasks that don't require targets, see [Registering maintenance window tasks without targets](maintenance-windows-targetless-tasks.md)\.

------

   The following example configures an automation as a registered task to a maintenance window with priority 1\. It also demonstrates omitting the `--targets`, `--max-errors`, and `--max-concurrency` options for a targetless maintenance window task\. The automation uses the `AWS-StartEC2Instance` runbook and the specified Automation assume role to start EC2 instances registered as targets to the maintenance window\. The maintenance window runs the automation simultaneously on 5 instances maximum at any given time\. Also, the registered task stops running on more instances for a particular interval if the error count exceeds 1\.

------
#### [ Linux & macOS ]

   ```
   aws ssm register-task-with-maintenance-window \
       --window-id mw-0c50858d01EXAMPLE \
       --name StartEC2Instances \
       --task-arn AWS-StartEC2Instance \
       --service-role-arn arn:aws:iam::123456789012:role/MaintenanceWindowRole \
       --task-type AUTOMATION \
       --task-invocation-parameters "{\"Automation\":{\"Parameters\":{\"InstanceId\":[\"{{TARGET_ID}}\"],\"AutomationAssumeRole\":[\"arn:aws:iam::123456789012:role/AutomationAssumeRole\"]}}}" \
       --priority 1
   ```

------
#### [ Windows ]

   ```
   aws ssm register-task-with-maintenance-window ^
       --window-id mw-0c50858d01EXAMPLE ^
       --name StartEC2Instances ^
       --task-arn AWS-StartEC2Instance ^
       --service-role-arn arn:aws:iam::123456789012:role/MaintenanceWindowRole ^
       --task-type AUTOMATION ^
       --task-invocation-parameters "{\"Automation\":{\"Parameters\":{\"InstanceId\":[\"{{TARGET_ID}}\"],\"AutomationAssumeRole\":[\"arn:aws:iam::123456789012:role/AutomationAssumeRole\"]}}}" ^
       --priority 1
   ```

------
#### [ PowerShell ]

   ```
   Register-SSMTaskWithMaintenanceWindow `
       -WindowId mw-0c50858d01EXAMPLE `
       -Name "StartEC2" `
       -TaskArn "AWS-StartEC2Instance" `
       -ServiceRoleArn "arn:aws:iam::123456789012:role/MaintenanceWindowRole" `
       -TaskType "AUTOMATION" `
       -Automation_Parameter @{ "InstanceId"="{{TARGET_ID}}";"AutomationAssumeRole"="arn:aws:iam::123456789012:role/AutomationAssumeRole" } `
       -Priority 1
   ```

------

   The command returns details for the new registered task similar to the following\.

------
#### [ Linux & macOS ]

   ```
   {
       "WindowTaskId": "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE"
   }
   ```

------
#### [ Windows ]

   ```
   {
       "WindowTaskId": "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE"
   }
   ```

------
#### [ PowerShell ]

   ```
   4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE
   ```

------

1. To view the registered task, run the following command\. Replace *maintenance windows ID* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-maintenance-window-tasks \
       --window-id maintenance window ID
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-maintenance-window-tasks ^
       --window-id maintenance window ID
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMMaintenanceWindowTaskList `
       -WindowId maintenance window ID
   ```

------

   The system returns information like the following\.

------
#### [ Linux & macOS ]

   ```
   {
       "Tasks": [
           {
               "ServiceRoleArn": "arn:aws:iam::123456789012:role/MaintenanceWindowRole",
               "MaxErrors": "1",
               "TaskArn": "AWS-StartEC2Instance",
               "MaxConcurrency": "1",
               "WindowTaskId": "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE",
               "TaskParameters": {},
               "Priority": 1,
               "WindowId": "mw-0c50858d01EXAMPLE",
               "Type": "AUTOMATION",
               "Targets": [
               ],
               "Name": "StartEC2"
           }
       ]
   }
   ```

------
#### [ Windows ]

   ```
   {
       "Tasks": [
           {
               "ServiceRoleArn": "arn:aws:iam::123456789012:role/MaintenanceWindowRole",
               "MaxErrors": "1",
               "TaskArn": "AWS-StartEC2Instance",
               "MaxConcurrency": "1",
               "WindowTaskId": "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE",
               "TaskParameters": {},
               "Priority": 1,
               "WindowId": "mw-0c50858d01EXAMPLE",
               "Type": "AUTOMATION",
               "Targets": [
               ],
               "Name": "StartEC2"
           }
       ]
   }
   ```

------
#### [ PowerShell ]

   ```
   Description    : 
   LoggingInfo    : 
   MaxConcurrency : 5
   MaxErrors      : 1
   Name           : StartEC2
   Priority       : 1
   ServiceRoleArn : arn:aws:iam::123456789012:role/MaintenanceWindowRole
   Targets        : {}
   TaskArn        : AWS-StartEC2Instance
   TaskParameters : {}
   Type           : AUTOMATION
   WindowId       : mw-0c50858d01EXAMPLE
   WindowTaskId   : 4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE
   ```

------