# Running Commands from the Console<a name="rc-console"></a>

You can use Run Command from the console to configure instances without having to login to each instance\. This topic includes an example that shows how to [update SSM Agent](#rc-console-agentexample) on an instance by using Run Command\.

**Before You Begin**  
Before you send a command using Run Command, verify that your instances meet Systems Manager [requirements](systems-manager-prereqs.md)\.

**To send a command using Run Command**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. In the **Command document** list, choose a Systems Manager document\.

1. In the **Targets** section, identify the instances on which you want to run this operation by specifying tags, selecting instances manually, or specifying a resource group\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. In the **Command parameters** section, specify values for required parameters\.

1. For **Other parameters**:
   + For **Comment**, type information about this command\.
   + For **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. \(Optional\) For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed instances or by specifying AWS resource groups, and you are not certain how many instances are targeted, then restrict the number of instances that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Instances still processing the command might also send errors\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an Amazon S3 bucket** box\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM Instance Profile for Systems Manager](setup-instance-profile.md)\.

1. In the **SNS notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Monitoring Systems Manager Status Changes Using Amazon SNS Notifications](monitoring-sns-notifications.md)\.

1. Choose **Run**\.

For information about canceling a command, see [Canceling a Command](rc-cancel.md)\. 

## Update SSM Agent by using Run Command<a name="rc-console-agentexample"></a>

The following procedure describes how to quickly update SSM Agent running on your Windows and Linux instances\. You can update to either the latest version or downgrade to an older version\. When you run the command, the system downloads the version from AWS, installs it, and then uninstalls the version that existed before the command was run\. If an error occurs during this process, the system rolls back to the version on the server before the command was run and the command status shows that the command failed\.

**Note**  
To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

**To update SSM Agent using Run Command**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. In the **Command document** list, choose **AWS\-UpdateSSMAgent**\.

1. In the **Targets** section, identify the instances on which you want to run this operation by specifying tags, selecting instances manually, or specifying a resource group\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. In the **Command parameters** section, specify values for the following parameters, if you want:

   1. \(Optional\) For **Version**, type the version of SSM Agent to install\. You can install [older versions](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) of the agent\. If you do not specify a version, the service installs the latest version\.

   1. \(Optional\) For **Allow Downgrade**, choose **true** to install an earlier version of SSM Agent\. If you choose this option, you must specify the [earlier](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) version number\. Choose **false** to install only the newest version of the service\.

1. For **Other parameters**:
   + For **Comment**, type information about this command\.
   + For **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. \(Optional\) For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed instances or by specifying AWS resource groups, and you are not certain how many instances are targeted, then restrict the number of instances that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Instances still processing the command might also send errors\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an Amazon S3 bucket** box\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM Instance Profile for Systems Manager](setup-instance-profile.md)\.

1. In the **SNS notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Monitoring Systems Manager Status Changes Using Amazon SNS Notifications](monitoring-sns-notifications.md)\.

1. Choose **Run**\.