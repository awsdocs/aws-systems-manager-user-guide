# Assign tasks to a maintenance window \(console\)<a name="sysman-maintenance-assign-tasks"></a>

In this procedure, you add a task to a maintenance window\. Tasks are the actions performed when a maintenance window runs\.

The following four types of tasks can be added to a maintenance window:
+ AWS Systems Manager Run Command commands
+ Systems Manager Automation workflows
+ AWS Lambda functions
**Important**  
The IAM policy for Maintenance Windows requires that you add the prefix `SSM` to Lambda function \(or alias\) names\. Before you proceed to register this type of task, update its name in AWS Lambda to include `SSM`\. For example, if your Lambda function name is `MyLambdaFunction`, change it to `SSMMyLambdaFunction`\.
+ AWS Step Functions tasks

**To assign tasks to a maintenance window**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\. 

1. In the list of maintenance windows, choose a maintenance window\.

1. Choose **Actions**, and then choose the option for the type of task you want to register with the maintenance window\.
   + **Register Run command task**
   + **Register Automation task**
   + **Register Lambda task**
   + **Register Step Functions task**

1. \(Optional\) For **Name**, enter a name for the task\.

1. \(Optional\) For **Description**, enter a description\.

1. For **New task invocation cutoff**, if you don't want any new task invocations to start after the maintenance window cutoff time is reached, choose **Enabled**\.

   When this option is *not* enabled, the task continues running when the cutoff time is reached and starts new task invocations until completion\. 
**Note**  
The status for tasks that are not completed when you enable this option is `TIMED_OUT`\. 

1. Do one of the following:
   + For a Run Command task: In the **Command document** list, choose the Systems Manager Command document \(SSM document\) that defines the tasks to run\. For **Document version**, choose the document version to use\.
   + For an Automation task: In the **Automation document** list, choose the Automation runbook that defines the tasks to run\. For **Document version**, choose the runbook version to use\.
   + For a Lambda task: In the **Lambda parameters** area, choose a Lambda function from the list\. \(Optional\) Provide any content for **Payload**, **Client Context**, or **Qualifier** that you want to include\.
   + For a Step Functions task: In the **Step Functions parameters** area, choose a state machine from the list\. \(Optional\) Provide a name for the state machine execution and any content for **Input** that you want to include\.

1. For **Task priority**, specify a priority for this task\. Zero \(`0`\) is the highest priority\. Tasks in a maintenance window are scheduled in priority order with tasks that have the same priority scheduled in parallel\.

1. In the **Targets** area, choose one of the following:
   + **Selecting registered target groups**: Select one or more maintenance window targets you have registered with the current maintenance window\.
   + **Selecting unregistered targets**: Choose available resources one by one as targets for the task\.

     If a managed node you expect to see isn't listed, see [Troubleshooting managed node availability](troubleshooting-managed-instances.md) for troubleshooting tips\.
   + **Task target not required**: Targets for the task might already be specified in other functions for all but Run Command\-type tasks\.

     Specify one or more targets for maintenance window Run Command\-type tasks\. Depending on the task, targets are optional for other maintenance window task types \(Automation, AWS Lambda, and AWS Step Functions\)\. For more information about running tasks that don't specify targets, see [Registering maintenance window tasks without targets](maintenance-windows-targetless-tasks.md)\.
**Note**  
In many cases, you don't need to explicitly specify a target for an automation task\. For example, say that you're creating an Automation\-type task to update an Amazon Machine Image \(AMI\) for Linux using the `AWS-UpdateLinuxAmi` runbook\. When the task runs, the AMI is updated with the latest available Linux distribution packages and Amazon software\. New instances created from the AMI already have these updates installed\. Because the ID of the AMI to be updated is specified in the input parameters for the runbook, there is no need to specify a target again in the maintenance window task\.

1. Automation tasks only:

   In the **Input parameters** area, provide values for any required or optional parameters needed to run your task\.

1. For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of managed nodes on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed nodes or by specifying AWS resource groups, and you aren't certain how many managed nodes are targeted, then restrict the number of targets that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other managed nodes after it fails on either a number or a percentage of nodes\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Managed nodes still processing the command might also send errors\.

1. In the ** IAM service role** area, choose one of the following options to provide permissions for Systems Manager to run tasks on your target nodes:
   +  ** Create and use a service\-linked role for Systems Manager **

     Service\-linked roles provide a secure way to delegate permissions to AWS services because only the linked service can assume a service\-linked role\. Additionally, AWS automatically defines and sets the permissions of service\-linked roles, depending on the actions that the linked service performs on your behalf\.
**Note**  
If a service\-linked role has already been created for your account, choose **Use the service\-linked role for Systems Manager**\.
   + **Use a custom service role**

     You can create a custom service role for maintenance window tasks if you want to use stricter permissions than those provided by the service\-linked role\. 

     If you need to create a custom service role, see one of the following topics:
     + [Control access to maintenance windows \(console\)](sysman-maintenance-perm-console.md)
     + [Control access to maintenance windows \(AWS CLI\)](sysman-maintenance-perm-cli.md)
     + [Control access to maintenance windows \(Tools for Windows PowerShell\)](sysman-maintenance-perm-ps.md)

   To help you decide whether to use a custom service role or the Systems Manager service\-linked role with a maintenance window task, see [Should I use a service\-linked role or a custom service role to run maintenance window tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)\.

1. Run Command tasks only:

   \(Optional\) For **Output options**, do the following:
   + Select the **Enable writing to S3** check box to save the command output to a file\. Enter the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the node, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. In addition, if the specified S3 bucket is in a different AWS account, verify that the instance profile associated with the node has the necessary permissions to write to that bucket\.
   + Select the **CloudWatch output** check box to write complete output to Amazon CloudWatch Logs\. Enter the name of a CloudWatch Logs log group\.

1. Run Command tasks only:

   In the **SNS notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

1. Run Command tasks only:

   In the **Parameters** area, specify parameters for the document\. 

1. Choose **Register task\.**