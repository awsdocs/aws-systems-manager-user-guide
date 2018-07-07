# Assign Tasks to a Maintenance Window \(Console\)<a name="sysman-maintenance-assign-tasks"></a>

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
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

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