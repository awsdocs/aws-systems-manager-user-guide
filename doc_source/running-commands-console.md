# Running commands from the console<a name="running-commands-console"></a>

You can use Run Command, a capability of AWS Systems Manager, from the AWS Management Console to configure managed nodes without having to log into them\. This topic includes an example that shows how to [update SSM Agent](run-command-tutorial-update-software.md#rc-console-agentexample) on a managed node by using Run Command\.

**Before you begin**  
Before you send a command using Run Command, verify that your managed nodes meet Systems Manager [requirements](systems-manager-prereqs.md)\.

**To send a command using Run Command**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose a Systems Manager document\.

1. In the **Command parameters** section, specify values for required parameters\.

1. In the **Targets** section, choose the managed nodes on which you want to run this operation by specifying tags, selecting instances or edge devices manually, or specifying a resource group\.
**Note**  
If a managed node you expect to see isn't listed, see [Troubleshooting managed node availability](troubleshooting-managed-instances.md) for troubleshooting tips\.

1. For **Other parameters**:
   + For **Comment**, enter information about this command\.
   + For **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of managed nodes on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed nodes or by specifying AWS resource groups, and you aren't certain how many managed nodes are targeted, then restrict the number of targets that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other managed nodes after it fails on either a number or a percentage of nodes\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Managed nodes still processing the command might also send errors\.

1. \(Optional\) Choose a CloudWatch alarm to apply to your command for monitoring\. To attach a CloudWatch alarm to your command, the IAM principal that runs the command must have permission for the `iam:createServiceLinkedRole` action\. For more information about CloudWatch alarms, see [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)\. Note that if your alarm activates, any pending command invocations do not run\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an S3 bucket** box\. Enter the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile \(for EC2 instances\) or IAM service role \(on\-premises machines\) assigned to the instance, not those of the IAM user performing this task\. For more information, see [Configure instance permissions for Systems Manager](setup-instance-permissions.md) or [Create an IAM service role for a hybrid environment](sysman-service-role.md)\. In addition, if the specified S3 bucket is in a different AWS account, make sure that the instance profile or IAM service role associated with the managed node has the necessary permissions to write to that bucket\.

1. In the **SNS notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

1. Choose **Run**\.

For information about canceling a command, see [Canceling a command](cancel-run-command.md)\. 

## Rerunning commands<a name="run-command-rerun"></a>

Systems Manager includes two options to help you rerun a command from the **Run Command** page in the Systems Manager console\. 
+ **Rerun**: This button allows you to run the same command without making changes to it\.
+ **Copy to new**: This button copies the settings of one command to a new command and gives you the option to edit those settings before you run it\.

**To rerun a command**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose a command to rerun\. You can rerun a command immediately after executing it from the command details page\. Or, you can choose a command that you previously ran from the **Command history** tab\.

1. Choose either **Rerun** to run the same command without changes, or choose **Copy to new** to edit the command settings before you run it\.