# Assign Tasks to a Maintenance Window \(Console\)<a name="sysman-maintenance-assign-tasks"></a>

In this procedure, you add a task to a maintenance window\. Tasks are the actions performed on a resource during a maintenance window execution\. 

The following four types of tasks can be added to a maintenance window:
+ Systems Manager Run Command commands
+ Systems Manager Automation workflows
+ AWS Lambda functions
+ AWS Step Functions tasks

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To assign tasks to a maintenance window \(AWS Systems Manager\)**

1. In the list of maintenance windows, choose a maintenance window \.

1. Choose **Actions** and then, choose either **Register run command task** to run your choice of commands on targets by using an SSM document, or **Register automation task** to run your choice of an Automation workflow on targets by using an SSM Automation document\. For examples of how to create Lambda and Step Functions tasks by using the AWS CLI, see the [Systems Manager Maintenance Windows Tutorials \(AWS CLI\)](maintenance-windows-tutorials.md)\.

1. For **Name**, enter a name for the task\.

1. For **Description**, enter a description\.

1. For **Document**, choose the SSM Command or Automation document that defines the tasks to run\.

1. For **Document version** \(for Automation tasks\), choose the document version to use\.

1. For **Task priority**, specify a priority for this task\. 1 is the highest priority\. Tasks in a maintenance window are scheduled in priority order with tasks that have the same priority scheduled in parallel\.

1. In the **Targets** section, identify the instances on which you want to run this operation by specifying tags or selecting instances manually\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. \(Optional\) For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by choosing Amazon EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Instances still processing the command might also send errors\.

1. In the ** IAM service role** area, choose one of the following options to provide permissions for Systems Manager to run tasks on your target instances:
   +  ** Create and use a service\-linked role for Systems Manager **

     Service\-linked roles provide a secure way to delegate permissions to AWS services because only the linked service can assume a service\-linked role\. Additionally, AWS automatically defines and sets the permissions of service\-linked roles, depending on the actions that the linked service performs on your behalf\.
**Note**  
If a service\-linked role has already been created for your account, choose **Use the service\-linked role for Systems Manager**\.
   + **Use a custom service role**

     You can create a custom service role for maintenance window tasks if you want to use stricter permissions than those provided by the service\-linked role\. Or you can create a custom service role if you want to use Amazon SNS to send notifications related to maintenance window tasks run through Run Command

     If you need to create a custom service role, see one of the following topics:
     + [Control Access to Maintenance Windows \(Console\)](sysman-maintenance-perm-console.md)
     + [Control Access to Maintenance Windows \(AWS CLI\)](sysman-maintenance-perm-cli.md)
     + [Control Access to Maintenance Windows \(Tools for Windows PowerShell\)](sysman-maintenance-perm-ps.md)

   To help you decide whether to use a custom service role or the Systems Manager service\-linked role with a maintenance window task, see [Should I Use a Service\-Linked Role or a Custom Service Role to Run Maintenance Window Tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)\.

1. In the **Input Parameters** section, specify parameters for the document\. For Automation documents, the system auto\-populates some of the values\. You can keep or replace these values\.

1. Complete the wizard\.

**To assign tasks to a maintenance window \(Amazon EC2 Systems Manager\)**

1. In the list of maintenance windows, choose the maintenance window you just created\.

1. Choose **Actions** and then, choose either **Register run command task** to run your choice of commands on targets by using an SSM document, or **Register automation task** to run your choice of an Automation workflow on targets by using an SSM Automation document\. For examples of how to create Lambda and Step Functions tasks by using the AWS CLI, see the [Systems Manager Maintenance Windows Tutorials \(AWS CLI\)](maintenance-windows-tutorials.md)\.

1. For **Task Name**, enter a name for the task\.

1. For **Description**, enter a description\.

1. For **Document**, choose the SSM Command or Automation document that defines the tasks to run\.

1. For **Document Version** \(for Automation tasks\), choose the document version to use\.

1. For **Task Priority**, specify a priority for this task\. 1 is the highest priority\. Tasks in a maintenance window are scheduled in priority order with tasks that have the same priority scheduled in parallel\.

1. In the **Target by** section, choose either **Selecting registered target groups** or **Selecting unregistered targets**, and then choose the targets\.

1. In the **Parameters** section, specify parameters for the document\. For Automation documents, the system auto\-populates some of the values\. You can keep or replace these values\.

   For **Role**, specify the maintenance windows ARN\. For more information about creating a maintenance windows ARN, see [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)\.

   **Execute on** lets you specify either a number of targets where the maintenance window tasks can run concurrently or a percentage of the total number of targets\. This field is relevant when you target a large number of instances using tags\. 

   **Stop after** lets you specify the number of allowed errors before the system stops sending the task to new instances\.

1. Complete the wizard\.