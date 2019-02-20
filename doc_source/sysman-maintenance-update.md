# Update or Delete a Maintenance Window \(Console\)<a name="sysman-maintenance-update"></a>

You can update or delete a Maintenance Window\. You can also update or delete the targets or tasks of a Maintenance Window\. If you edit the details of a Maintenance Window, you can change the schedule, targets, and tasks\. You can also specify names and descriptions for windows, targets, and tasks, which helps you better understand their purpose, and makes it easier to manage your queue of windows\.

This section describes how to update or delete a Maintenance Window, targets, and tasks by using the AWS Systems Manager console\. For examples of how to do this by using the AWS CLI, see [Tutorial: Update a Maintenance Window \(AWS CLI\)](maintenance-windows-cli-tutorials-update.md)\. 

**Topics**
+ [Update or Delete a Maintenance Window \(Console\)](#sysman-maintenance-update-mw)
+ [Update or Delete Maintenance Window Targets \(Console\)](#sysman-maintenance-update-target)
+ [Update or Delete Maintenance Window Tasks \(Console\)](#sysman-maintenance-update-tasks)

## Update or Delete a Maintenance Window \(Console\)<a name="sysman-maintenance-update-mw"></a>

You can update a Maintenance Window to change its name, description, and schedule, and whether the Maintenance Window should allow unregistered targets\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To update or delete a Maintenance Window \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\. 

1. Choose the Maintenance Window that you want to update or delete, and then do one of the following:
   + Choose **Delete**\. The system prompts you to confirm your actions\. 
   + Choose **Edit**\. On the **Edit maintenance window** page, change the values and options that you want, and then choose **Edit maintenance window**\.

     For information about the configuration choices you can make, see [Create a Maintenance Window \(Console\)](sysman-maintenance-create-mw.md)\.

**To update or delete a Maintenance Window \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Shared Resources** in the navigation pane, and then choose **Maintenance Windows**\. 

1. Choose the Maintenance Window that you want to update or delete\.

1. Choose **Actions**, and then choose either **Delete Maintenance Window** or **Edit maintenance window**\. If you chose to delete a Maintenance Window the system prompts you to confirm your actions\. If you chose to edit a Maintenance Window, the **Edit maintenance window** page appears\.

1. Change the values and options that you want, and then choose **Edit maintenance window**\. The system returns you to the Maintenance Window page\.

   For information about options you can choose, see [Create a Maintenance Window \(Console\)](sysman-maintenance-create-mw.md)\.

## Update or Delete Maintenance Window Targets \(Console\)<a name="sysman-maintenance-update-target"></a>

You can update or delete the targets of a Maintenance Window\. If you choose to update a Maintenance Window target you can specify a new target name, description, and owner\. You can also choose different targets\. 

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To update or delete the targets of a Maintenance Window \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\. 

1. Choose the name of the Maintenance Window that you want to update, and then do one of the following:
   + To update targets, choose **Edit**\.
   + To delete targets, choose Deregister targets, and then choose the **Targets** tab\.

     Choose the target to delete, and then choose **Deregister target**\. In the **Deregister maintenance windows target** window, leave the **Safely deregister target** option selected if you want the system to check if the target is referenced by any tasks before deleting it\. If the target is referenced by a task, the system returns an error and doesn't delete the target\. Clear the **Safely deregister target** option if you want the system to delete the target even if it is referenced by a task\.

     Choose **Deregister**\.

**To update or delete the targets of a Maintenance Window \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Shared Resources** in the navigation pane, and then choose **Maintenance Windows**\. 

1. Choose the Maintenance Window that you want to update\.

1. Choose the **Targets** tab\.

1. If you want to delete a target, choose the small X beside **Edit**\. In the **Deregister target** window, leave the **Safely Deregister Target** option selected if you want the system to check if the target is referenced by any tasks before deleting it\. If the target is referenced by a task, the system returns an error and doesn't delete the target\. Clear the **Safely Deregister Target** option if you want the system to delete the target even if it is referenced by a task\.

   If you want to edit the targets, choose **Edit**\.

1. Change the values and options that you want, and then choose **Edit Target**\. The system returns you to the Maintenance Window page\.

## Update or Delete Maintenance Window Tasks \(Console\)<a name="sysman-maintenance-update-tasks"></a>

You can update or delete the tasks of a Maintenance Window\. If you choose to update, you can specify a new task name, description, and owner\. For Run Command and Automation tasks, you can choose a different SSM document for the tasks\. You can't, however, edit a task to change its type\. For example, if you created an Automation task, you can't edit that task and change it to a Run Command task\. 

**Note**  
The following procedure describes steps that you perform in the Amazon EC2 console\. You can also perform these steps in the new [AWS Systems Manager console](https://console.aws.amazon.com/systems-manager/)\. The steps in the new console will differ from the steps below\.

**To update or delete the tasks of a Maintenance Window \(console\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Shared Resources** in the navigation pane, and then choose **Maintenance Windows**\. 

1. Choose the Maintenance Window that you want to update\.

1. Choose the **Tasks** tab\.

1. If you want to delete a task, choose the small **x** button beside **Edit**\. If you want to edit the task, choose **Edit**\.

1. Change the values and options that you want, and then choose **Edit Task**\. The system returns you to the Maintenance Window page\.