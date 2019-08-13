# Running Automation Workflows with Triggers using a Maintenance Window<a name="automation-mw-target"></a>

You can start an Automation workflow by configuring an Automation document as a registered task for a maintenance window\. By registering the Automation document as a registered task, the maintenance window runs the automation workflow during the scheduled maintenance period\. 

For example, let's say you create an Automation document named *CreateAMI* that creates an Amazon Machine Image \(AMI\) of instances registered as targets to the maintenance window\. To specify the *CreateAMI* document \(and corresponding workflow\) as a registered task of a maintenance window, you first create a maintenance window and register targets\. Then you use the following procedure to specify the *CreateAMI* document as a registered task within the maintenance window\. When the maintenance window starts during the scheduled period, the system runs the automation workflow and creates an AMI of the registered targets\.

For information about creating Automation documents, see [Working with Automation Documents](automation-documents.md)\.

Use the following procedure to configure an Automation workflow as a registered task for a maintenance window\.

**Before You Begin**  
Before you complete the following procedure, you must create a maintenance window and register at least one target\. For more information, see the following procedures: 
+ [Create a Maintenance Window \(Console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-maintenance-create-mw.html)\.
+ [Assign Targets to a Maintenance Window \(Console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-maintenance-assign-targets.html)

**To configure an Automation workflow as a registered task for a maintenance window**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the left navigation pane, choose **Maintenance Windows**, and then choose the maintenance window you want to register an Automation task with\.

1. Choose **Actions**\. Then choose **Register Automation task** to run your choice of an Automation workflow on targets by using an Automation document\.

1. For **Name**, enter a name for the task\.

1. For **Description**, enter a description\.

1. For **Document**, choose the Automation document that defines the tasks to run\.

1. For **Document version**, choose the document version to use\.

1. For **Task priority**, specify a priority for this task\. 1 is the highest priority\. Tasks in a maintenance window are scheduled in priority order; tasks that have the same priority are scheduled in parallel\.

1. In the **Targets** section, identify the targets on which you want to run this automation workflow by specifying tags or by selecting instances manually\.
**Important**  
If you choose an Automation document that doesn't target managed instances, you must still select at least one maintenance window target\. In this situation, we recommend registering a target for a tag key\-value pair that is not used by your managed instances\.   
For example, if you choose the Automation document `AWS-CopySnapshot`, then the resulting automation workflow targets Amazon Elastic Block Store \(EBS\) snapshots instead of managed instances\. In this case, you can register a target to your maintenance window, which targets a tag key\-value pair that is not used by your managed instances, such as key=MaintenanceWindow and value=Snapshot\.

1. \(Optional\) For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of targets on which to run the automation workflow at the same time\.
**Note**  
If you selected targets by choosing tag key\-value pairs, and you are not certain how many targets use the selected tags, then limit the number of automation workflows that can run at the same time by specifying a percentage\.  
When the maintenance window runs, a new Automation execution is initiated per target\. There is a limit of 25 concurrent executions of Automation per AWS account\. If you specify a concurrency rate greater than 25, concurrent executions greater than 25 are automatically added to the execution queue\. For information, see [AWS Systems Manager Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_ssm)\. 
   + For **Error threshold**, specify when to stop running the automation workflow on other targets after it fails on either a number or a percentage of targets\. For example, if you specify three errors, then Systems Manager stops running automation workflows when the third error is received\. Targets still processing the workflow might also send errors\.

1. In the ** IAM service role** area, choose one of the following options to provide permissions for Systems Manager to start the Automation workflow:
   +  ** Create and use a service\-linked role for Systems Manager **

     Service\-linked roles provide a secure way to delegate permissions to AWS services because only the linked service can assume a service\-linked role\. Additionally, AWS automatically defines and sets the permissions of service\-linked roles, depending on the actions that the linked service performs on your behalf\.
**Note**  
If a service\-linked role has already been created for your account, choose **Use the service\-linked role for Systems Manager**\.
   + **Use a custom service role**

     If you want to use stricter permissions than those provided by the service\-linked role, you can create a custom service role for maintenance window tasks\. If you want to use Amazon SNS to send notifications related to maintenance window tasks run through Run Command, you can create a custom service role\.

     To create a custom service role, see one of the following topics:
     + [Control Access to Maintenance Windows \(Console\)](sysman-maintenance-perm-console.md)
     + [Control Access to Maintenance Windows \(AWS CLI\)](sysman-maintenance-perm-cli.md)
     + [Control Access to Maintenance Windows \(Tools for Windows PowerShell\)](sysman-maintenance-perm-ps.md)

   To help you decide whether to use a custom service role or the Systems Manager service\-linked role with a maintenance window task, see [Should I Use a Service\-Linked Role or a Custom Service Role to Run Maintenance Window Tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)\.

1. In the **Input Parameters** section, specify parameters for the document\. For Automation documents, the system auto\-populates some of the values\. You can keep or replace these values\.
**Important**  
For Automation documents, you can optionally specify an Automation Assume Role\. If you don't specify a role for this parameter, then the Automation workflow assumes the maintenance window service role you choose in step 11\. As such, you must ensure that the maintenance window service role you choose has the appropriate AWS Identity and Access Management \(IAM\) permissions to perform the actions defined within the Automation document\.   
For example, the service\-linked role for Systems Manager doesn't have the IAM permission `ec2:CreateSnapshot`, which is required to run the Automation document `AWS-CopySnapshot`\. In this scenario, you must either use a custom maintenance window service role or specify an Automation assume role that has `ec2:CreateSnapshot` permissions\. For information, see [Getting Started with Automation](automation-setup.md)\.

1. Choose **Register Automation task**\.