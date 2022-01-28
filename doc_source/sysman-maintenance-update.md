# Updating or deleting maintenance window resources \(console\)<a name="sysman-maintenance-update"></a>

You can update or delete a maintenance window in Maintenance Windows, a capability of AWS Systems Manager\. You can also update or delete the targets or tasks of a maintenance window\. If you edit the details of a maintenance window, you can change the schedule, targets, and tasks\. You can also specify names and descriptions for windows, targets, and tasks, which helps you better understand their purpose, and makes it easier to manage your queue of windows\.

This section describes how to update or delete a maintenance window, targets, and tasks by using the Systems Manager console\. For examples of how to do this by using the AWS Command Line Interface \(AWS CLI\), see [Tutorial: Update a maintenance window \(AWS CLI\)](maintenance-windows-cli-tutorials-update.md)\. 

**Topics**
+ [Updating or deleting a maintenance window \(console\)](#sysman-maintenance-update-mw)
+ [Updating or deregistering maintenance window targets \(console\)](#sysman-maintenance-update-target)
+ [Updating or deregistering maintenance window tasks \(console\)](#sysman-maintenance-update-tasks)

## Updating or deleting a maintenance window \(console\)<a name="sysman-maintenance-update-mw"></a>

You can update a maintenance window to change its name, description, and schedule, and whether the maintenance window should allow unregistered targets\.

**To update or delete a maintenance window**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\. 

1. Select the button next to the maintenance window that you want to update or delete, and then do one of the following:
   + Choose **Delete**\. The system prompts you to confirm your actions\. 
   + Choose **Edit**\. On the **Edit maintenance window** page, change the values and options that you want, and then choose **Save changes**\.

     For information about the configuration choices you can make, see [Create a maintenance window \(console\)](sysman-maintenance-create-mw.md)\.

## Updating or deregistering maintenance window targets \(console\)<a name="sysman-maintenance-update-target"></a>

You can update or deregister the targets of a maintenance window\. If you choose to update a maintenance window target you can specify a new target name, description, and owner\. You can also choose different targets\. 

**To update or delete the targets of a maintenance window**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\. 

1. Choose the name of the maintenance window that you want to update, choose the **Targets** tab, and then do one of the following:
   + To update targets, select the button next to the target to update, and then choose **Edit**\.
   + To deregister targets, select the button next to the target to deregister, and then choose **Deregister target**\. In the **Deregister maintenance windows target** dialog box, choose **Deregister**\.

## Updating or deregistering maintenance window tasks \(console\)<a name="sysman-maintenance-update-tasks"></a>

You can update or deregister the tasks of a maintenance window\. If you choose to update, you can specify a new task name, description, and owner\. For Run Command and Automation tasks, you can choose a different SSM document for the tasks\. You can't, however, edit a task to change its type\. For example, if you created an Automation task, you can't edit that task and change it to a Run Command task\. 

**To update or delete the tasks of a maintenance window \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\. 

1. Choose the name of the maintenance window that you want to update\.

1. Choose the **Tasks** tab, and then select the button next to the task to update\.

1. Do one of the following:
   + To deregister a task, choose **Deregister task**\.
   + To edit the task, choose **Edit**\. Change the values and options that you want, and then choose **Edit task**\.