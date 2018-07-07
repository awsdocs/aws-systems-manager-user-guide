# Running Commands from the Console<a name="rc-console"></a>

You can use Run Command from the console to configure instances without having to login to each instance\. This topic includes an example that shows how to [update SSM Agent](#rc-console-agentexample) on an instance by using Run Command\.

**Before You Begin**  
Before you send a command using Run Command, verify that your instances meet Systems Manager [requirements](systems-manager-prereqs.md)\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To send a command using Run Command \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. In the **Command document** list, choose a Systems Manager document\.

1. In the **Targets** section, identify the instances where you want to run this operation by specifying tags or selecting instances manually\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. In the **Command parameters** section, specify values for required parameters\.

1. In **Other parameters**:
   + In the **Comment** box, type information about this command\.
   + In **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. \(Optional\) In **Rate control**:
   + In **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by choosing Amazon EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document at the same time by specifying a percentage\.
   + In **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify 3 errors, then Systems Manager stops sending the command when the 4th error is received\. Instances still processing the command might also send errors\.

1. In the **Output options** section, if you want to save the command output to a file, select the **Write command output to an Amazon S3 bucket**\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\. 

1. In the **SNS Notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.

1. Choose **Run**\.

For information about canceling a command, see [Canceling a Command](rc-cancel.md)\. 

**To send a command using Run Command \(Amazon EC2 Systems Manager\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, expand **Systems Manager Services**, and then choose **Run Command**\.

1. Choose **Run a command**\.

1. In the **Command document** section, choose a document\.

1. In the **Select Targets by** section, choose **Manually Selecting Instances** to chose individual instances\. Or choose **Specifying a Tag**, choose a group of instances by specifying one or more Amazon EC2 tags\.

1. In the **Execute on** field, choose either **Targets** or **Percent** in the list\. If you choose **Targets**, then you can specify the exact number of instances that should be allowed to run the command at one time, for example, 10\. If you choose **Percent**, then you can choose a percentage of the instances that should be allowed to run the command at one time, for example 30\. **Percent** is a helpful option when targeting EC2 tags and you are not certain of the total number of instances that will run the command\.

   This feature allows you to limit the number of instances running the command at one time to avoid impacting instance performance and availability\. For more information, see [Sending Commands to a Fleet](send-commands-multiple.md)\.

1. In the **Stop after \_\_ errors** field, specify the maximum number of errors allowed before the system stops sending the command to additional instances\. For example, if you specify 1, then the systems stops sending the command to additional instances when the system receives the second error\.

   Instances that are already running a command when this value is reached are allowed to complete, but some of these executions may fail as well\. For more information, see [Sending Commands to a Fleet](send-commands-multiple.md)\.

1. In the next section, specify the parameters or options for your SSM document\. Parameters and options are different for each document\.

1. For **Comment**, we recommend providing information to will help you identify this command in your list of commands\.

1. For **Timeout \(seconds\)**, type the number of seconds that Run Command should attempt to reach an instance before it is considered unreachable and the command execution fails\. The minimum is 30 seconds, the maximum is 30 days, and the default is 10 minutes\.

1. \(Optional\) Choose **Write output to an S3 bucket** if you want to write the command output to an Amazon S3 bucket\. If you chose this option, specify the S3 bucket and, optionally, an S3 key prefix\. An S3 key prefix is a subfolder in the S3 bucket\. A subfolder can help you organize Run Command output if you run multiple commands against multiple instances\.
**Important**  
The Run Command **Output** page in the Amazon EC2 console truncates output after 2500 characters\. Configure an Amazon S3 bucket before executing commands using Run Command\. If your command output was longer than 2500 characters, you can view the full output in your Amazon S3 bucket\. For more information, see [Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)\.

1. \(Optional\) Choose **Enable SNS notifications** if you want to receive notifications about the status of the commands you run with Run Command\. For more information, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.
**Note**  
After you specify parameters and options for your SSM document, expand the **AWS Command Line Interface command** section\. This section includes a reusable command for different command\-line platforms\.

1. Choose **Run**, and then choose **View results**\.

1. In the commands list, choose the command you just ran\. If the command is still in progress, choose the refresh icon in the top right corner of the console\. 

1. When the **Status** column shows **Success** or **Failed**, choose the **Output** tab\.

1. Choose **View Output**\. The command output page shows the results of your command execution\.

For information about canceling a command, see [Canceling a Command](rc-cancel.md)\. 

## Example: Update the SSM Agent<a name="rc-console-agentexample"></a>

You can use the AWS\-UpdateSSMAgent document to update the Amazon EC2 SSM Agent running on your Windows and Linux instances\. You can update to either the latest version or downgrade to an older version\. When you run the command, the system downloads the version from AWS, installs it, and then uninstalls the version that existed before the command was run\. If an error occurs during this process, the system rolls back to the version on the server before the command was run and the command status shows that the command failed\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To send a command using Run Command \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. In the **Command document** list, choose **AWS\-UpdateSSMAgent**\.

1. In the **Targets** section, identify the instances where you want to run this operation by specifying tags or selecting instances manually\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. In the **Command parameters** section, specify values for the following parameters, if you want:

   1. \(Optional\) For **Version**, type the version of SSM Agent to install\. You can install [older versions](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) of the agent\. If you do not specify a version, the service installs the latest version\.

   1. \(Optional\) For **Allow Downgrade**, choose **true** to install an earlier version of SSM Agent\. If you choose this option, you must specify the [earlier](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) version number\. Choose **false** to install only the newest version of the service\.

1. In **Other parameters**:
   + In the **Comment** box, type information about this command\.
   + In **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. \(Optional\) In **Rate control**:
   + In **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by choosing Amazon EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document at the same time by specifying a percentage\.
   + In **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify 3 errors, then Systems Manager stops sending the command when the 4th error is received\. Instances still processing the command might also send errors\.

1. In the **Output options** section, if you want to save the command output to a file, select the **Write command output to an Amazon S3 bucket**\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\. 

1. In the **SNS Notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.

1. Choose **Run**\.

**To update SSM Agent using Run Command \(Amazon EC2 Systems Manager\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane under **Systems Manager Services**, choose **Run Command**\.

1. Choose **Run a command**\.

1. For **Command document**, choose **AWS\-UpdateSSMAgent**\.

1. In the **Select Targets by** section, choose **Manually Selecting Instances** to chose individual instances\. Or choose **Specifying a Tag** to choose a group of instances by specifying one or more Amazon EC2 tags\.

1. In the **Execute on** field, choose either **Targets** or **Percent** in the list\. If you choose **Targets**, then you can specify the exact number of instances that should be allowed to run the command at one time, for example, 10\. If you choose **Percent**, then you can choose a percentage of the instances that should be allowed to run the command at one time, for example 30\. **Percent** is a helpful option when targeting EC2 tags and you are not certain of the total number of instances that will run the command\.

   This feature allows you to limit the number of instances running the command at one time to avoid impacting instance performance and availability\. For more information, see [Sending Commands to a Fleet](send-commands-multiple.md)\.

1. In the **Stop after \_\_ errors** field, specify the maximum number of errors allowed before the system stops sending the command to additional instances\. For example, if you specify 1, then the systems stops sending the command to additional instances when the system receives the second error\. For more information, see [Sending Commands to a Fleet](send-commands-multiple.md)\.

1. \(Optional\) For **Version**, type the version of SSM Agent to install\. You can install [older versions](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) of the agent\. If you do not specify a version, the service installs the latest version\.

1. \(Optional\) For **Allow Downgrade**, choose **true** to install an earlier version of the SSM agent\. If you choose this option, you must specify the [earlier](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) version number\. Choose **false** to install only the newest version of the service\.

1. For **Comment**, we recommend providing information that will help you identify this command in your list of commands\.

1. For **Timeout \(seconds\)**, type the number of seconds that Run Command should attempt to reach an instance before it is considered unreachable and the command execution fails\. The minimum is 30 seconds, the maximum is 30 days, and the default is 10 minutes\.

1. \(Optional\) Choose **Write output to an S3 bucket** if you want to write the command output to an Amazon S3 bucket\. If you chose this option, specify the S3 bucket and, optionally, an S3 key prefix\. An S3 key prefix is a subfolder in the S3 bucket\. A subfolder can help you organize Run Command output if you run multiple commands against multiple instances\.
**Important**  
The Run Command **Output** page in the Amazon EC2 console truncates output after 2500 characters\. Configure an Amazon S3 bucket before executing commands using Run Command\. If your command output was longer than 2500 characters, you can view the full output in your Amazon S3 bucket\. For more information, see [Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)\.

1. \(Optional\) Choose **Enable SNS notifications** if you want to receive notifications about the status of the commands you run with Run Command\. For more information, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.
**Note**  
After you specify parameters and options for your SSM document, expand the **AWS Command Line Interface command** section\. This section includes a reusable command for different command\-line platforms\.

1. Choose **Run**, and then choose **View results**\.

1. In the commands list, choose the command you just ran\. If the command is still in progress, choose the refresh icon in the top right corner of the console\. 

1. When the **Status** column shows **Success** or **Failed**, choose the **Output** tab\.

1. Choose **View Output**\. The command output page shows the results of your command execution\.