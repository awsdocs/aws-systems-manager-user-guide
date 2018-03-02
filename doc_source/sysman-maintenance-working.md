# Working with Maintenance Windows<a name="sysman-maintenance-working"></a>

This section describes how to create, configure, and update or delete a Maintenance Window\. This section also describes how to perform these same tasks for the targets and tasks of a Maintenance Window\.

**Important**  
We recommend that you initially create and configure Maintenance Windows in a test environment\. 

**Before You Begin**  
Before you create a Maintenance Window, you must configure access to Maintenance Windows\. For more information, see [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)\.


+ [Create a Maintenance Window](#sysman-maintenance-create-mw)
+ [Assign Targets to a Maintenance Window](#sysman-maintenance-assign-targets)
+ [Assign Tasks to a Maintenance Window](#sysman-maintenance-assign-tasks)
+ [Update or Delete a Maintenance Window](#sysman-maintenance-update)

## Create a Maintenance Window<a name="sysman-maintenance-create-mw"></a>

To create a Maintenance Window, you must do the following:

+ Create the window and define its schedule and duration\.

+ Assign targets for the window\.

+ Assign tasks to run during the window\.

After you complete these steps, the Maintenance Window runs according to the schedule you defined and runs the tasks on the targets you specified\. After a task is finished, Systems Manager logs the details of the execution\. 

You can run the following types of tasks on targets:

+ Commands by using Systems Manager Run Command

+ Automation workflows by using Systems Manager Automation

+ Functions by using AWS Lambda

+ State machines by using AWS Step Functions
**Note**  
The AWS Systems Manager console does not currently support running Step Functions\. To register this type of task, you must use the AWS CLI\. For examples of how to create, configure, and update a Maintenance Window by using the AWS CLI, see the [Systems Manager Maintenance Window Walkthroughs](sysman-maintenance-walk.md)\.

**To create a Maintenance Window**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

   \-or\-

   Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.
**Note**  
If you are using the Amazon EC2 console, some field names and locations may differ slightly\.

1. In the navigation pane, choose **Maintenance Windows**\. 

1. Choose **Create a Maintenance Window**\.

1. In the **Name** field, type a descriptive name to help you identify this Maintenance Window as a test Maintenance Window\.

1. In the **Description** field, enter a description\.

1. Choose **Allow unregistered targets** if you want to allow a Maintenance Window task to run on managed instances, even if you have not registered those instances as targets\. If you choose this option, then you can choose the unregistered instances \(by instance ID\) when you register a task with the Maintenance Window\.

   If you don't choose this option, then you must choose previously\-registered targets when you register a task with the Maintenance Window\.

1. Specify a schedule for the Maintenance Window by using one of the scheduling options\.

1. In the **Duration** field, type the number of hours the Maintenance Window should run\.

1. In the **Stop initiating tasks** field, type the number of hours before the end of the Maintenance Window that the system should stop scheduling new tasks to run\.

1. Choose **Create maintenance window**\. The system returns you to the Maintenance Window page\. The state of the Maintenance Window you just created is **Enabled**\.

## Assign Targets to a Maintenance Window<a name="sysman-maintenance-assign-targets"></a>

After you create a Maintenance Window, you assign targets where the tasks will run\.

**To assign targets to a Maintenance Window**

1. In the Maintenance Window list, choose the Maintenance Window you just created\.

1. Choose **Actions**, and then choose **Register targets**\.

1. In the **Target Name** field, type a name for the targets\.

1. In the **Description** field, type a description\.

1. In the **Owner information** field, specify your name or work alias\. Owner information is included in any CloudWatch Events raised while running tasks for these targets in this Maintenance Window\. 

1. In the **Select targets by** section, choose **Specifying Tags** to target instances by using Amazon EC2 tags that you previously assigned to the instances\. Choose **Manually Selecting Instances** to choose individual instances according to their instance ID\.
**Note**  
If you don't see the instances that you'd like to target, verify that those instances are configured for Systems Manager\. For more information, see [Setting Up AWS Systems Manager](systems-manager-setting-up.md)\.

1. Choose **Register targets**\.

If you want to assign more targets to this window, choose the **Targets** tab, and then choose **Register new targets**\. With this option, you can choose a different means of targeting\. For example, if you previously targeted instances by instance ID, you can register new targets and target instances by specifying Amazon EC2 tags\.

## Assign Tasks to a Maintenance Window<a name="sysman-maintenance-assign-tasks"></a>

After you assign targets, you assign tasks to perform during the window\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To assign tasks to a Maintenance Window \(AWS Systems Manager\)**

1. In the Maintenance Window list, choose the Maintenance Window you just created\.

1. Choose **Actions** and then, choose either **Register run command task** to run your choice of commands on targets by using an SSM document, or **Register automation task** to run your choice of an Automation workflow on targets by using an SSM Automation document\. For examples of how to create Lambda and Step Functions tasks by using the AWS CLI, see the [Systems Manager Maintenance Window Walkthroughs](sysman-maintenance-walk.md)\.

1. In the **Name** field, type a name for the task\.

1. In the **Description** field, type a description\.

1. From the **Document** list, choose the SSM Command or Automation document that defines the task\(s\) to run\.

1. In the **Document version** list \(for Automation tasks\), choose the document version to use\.

1. In the **Task priority** list, specify a priority for this task\. 1 is the highest priority\. Tasks in a Maintenance Window are scheduled in priority order with tasks that have the same priority scheduled in parallel\.

1. In the **Targets** section, identify the instances where you want to run this operation by specifying tags or selecting instances manually\.

1. \(Optional\) In **Rate control**:

   + In **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by choosing Amazon EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document at the same time by specifying a percentage\.

   + In **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify 3 errors, then Systems Manager stops sending the command when the 4th error is received\. Instances still processing the command might also send errors\.

1. In the **IAM Role** field, specify the Maintenance Windows ARN\. For more information about creating a Maintenance Windows ARN, see [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)\.

1. In the **Input Parameters** section, specify parameters for the document\. For Automation documents, the system auto\-populates some of the values\. You can keep or replace these values\.

1. Complete the wizard\.

**To assign tasks to a Maintenance Window \(Amazon EC2 Systems Manager\)**

1. In the Maintenance Window list, choose the Maintenance Window you just created\.

1. Choose **Actions** and then, choose either **Register run command task** to run your choice of commands on targets by using an SSM document, or **Register automation task** to run your choice of an Automation workflow on targets by using an SSM Automation document\. For examples of how to create Lambda and Step Functions tasks by using the AWS CLI, see the [Systems Manager Maintenance Window Walkthroughs](sysman-maintenance-walk.md)\.

1. In the **Task Name** field, type a name for the task\.

1. In the **Description** field, type a description\.

1. From the **Document** list, choose the SSM Command or Automation document that defines the task\(s\) to run\.

1. In the **Document Version** list \(for Automation tasks\), choose the document version to use\.

1. In the **Task Priority** field, specify a priority for this task\. 1 is the highest priority\. Tasks in a Maintenance Window are scheduled in priority order with tasks that have the same priority scheduled in parallel\.

1. In the **Target by** section, choose either **Selecting registered target groups** or **Selecting unregistered targets**, and then choose the targets\.

1. In the **Parameters** section, specify parameters for the document\. For Automation documents, the system auto\-populates some of the values\. You can keep or replace these values\.

   In the **Role** field, specify the Maintenance Windows ARN\. For more information about creating a Maintenance Windows ARN, see [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)\.

   The **Execute on** field lets you specify either a number of targets where the Maintenance Window tasks can run concurrently or a percentage of the total number of targets\. This field is relevant when you target a large number of instances using tags\. 

   The **Stop after** field lets you specify the number of allowed errors before the system stops sending the task to new instances\.

1. Complete the wizard\.

## Update or Delete a Maintenance Window<a name="sysman-maintenance-update"></a>

You can update or delete a Maintenance Window\. You can also update or delete the targets or tasks of a Maintenance Window\. If you edit the details of a Maintenance Window, you can change the schedule, targets, and tasks\. You can also specify names and descriptions for windows, targets, and tasks, which helps you better understand their purpose, and makes it easier to manage your queue of windows\.

This section describes how to update or delete a Maintenance Window, targets, and tasks by using the AWS Systems Manager console\. For examples of how to do this by using the AWS CLI, see [Updating a Maintenance Window](sysman-maintenance-walk.md#sysman-mw-walk-update)\. 

### Updating or Deleting a Maintenance Window<a name="sysman-maintenance-update-mw"></a>

You can update a Maintenance Window to changethe name, description, and schedule of the window, and whether the window should allow unregistered targets\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To update or delete a Maintenance Window \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\. 

1. Choose the Maintenance Window that you want to update or delete, and then do one of the following:

   + Choose **Delete**\. The system prompts you to confirm your actions\. 

   + Choose **Edit**\. On the **Edit maintenance window** page, change the values and options that you want, and then choose **Edit maintenance window**\. 

**To update or delete a Maintenance Window \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Shared Resources** in the navigation pane, and then choose **Maintenance Windows**\. 

1. Choose the Maintenance Window that you want to update or delete\.

1. Choose **Actions**, and then choose either **Delete Maintenance Window** or **Edit maintenance window**\. If you chose to delete a Maintenance Window the system prompts you to confirm your actions\. If you chose to edit a Maintenance Window, the **Edit maintenance window** page appears\.

1. Change the values and options that you want, and then choose **Edit maintenance window**\. The system returns you to the Maintenance Window page\.

### Updating or Deleting the Targets of a Maintenance Window<a name="sysman-maintenance-update-target"></a>

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
![\[Updating the targets of a Maintenance Window\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/sysman-update-mw1.png)

1. Change the values and options that you want, and then choose **Edit Target**\. The system returns you to the Maintenance Window page\.

### Updating or Deleting the Tasks of a Maintenance Window<a name="sysman-maintenance-update-tasks"></a>

You can update or delete the tasks of a Maintenance Window\. If you choose to update, you can specify a new task name, description, and owner\. For Run Command and Automation tasks, you can choose a different SSM document for the tasks\. You can't, however, edit a task to change its type\. For example, if you created an Automation task, you can't edit that task and change it to a Run Command task\. 

**Note**  
The following procedure describes steps that you perform in the Amazon EC2 console\. You can also perform these steps in the new [AWS Systems Manager console](https://console.aws.amazon.com/systems-manager/)\. The steps in the new console will differ from the steps below\.

**To update or delete the tasks of a Maintenance Window**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Shared Resources** in the navigation pane, and then choose **Maintenance Windows**\. 

1. Choose the Maintenance Window that you want to update\.

1. Choose the **Tasks** tab\.

1. If you want to delete a task, choose the small X beside **Edit**\. If you want to edit the task, choose **Edit**\.  
![\[Updating the tasks of a Maintenance Window\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/sysman-update-mw2.png)

1. Change the values and options that you want, and then choose **Edit Task**\. The system returns you to the Maintenance Window page\.