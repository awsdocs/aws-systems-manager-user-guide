# Running PowerShell scripts on Linux instances<a name="powershell-run-command-linux"></a>

Using the `aws:runPowerShellScript` plugin or the `AWS-RunPowerShellScript` command document, along with PowerShell Core, you can run PowerShell scripts on Linux instances\. This can be useful for systems administrators who are familiar with PowerShell and prefer it to other scripting languages\.

**Before You Begin**  
Connect to your Linux instance and follow the [PowerShell Core ](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell-core-on-linux?view=powershell-6) installation procedure for the appropriate operating system\.

**Note**  
Many PowerShell commands \(cmdlets\) are not available on Linux\. To see which commands are available, use the `Get-Command` cmdlet after starting PowerShell using the `pwsh` command on your Linux instance\. For more information, see [Get\-Command](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-command?view=powershell-6)\. 

The following procedure describes how to run a PowerShell script on a Linux instance using the console\.

**To run a PowerShell script on a Linux instance using the console**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose the `AWS-RunPowerShellScript` document\.

1. In the **Command parameters** section, specify the available PowerShell commands you want to use\.

1. In the **Targets** section, identify the instances on which you want to run this operation by specifying tags, selecting instances manually, or specifying a resource group\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Some of my instances are missing](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. For **Other parameters**:
   + For **Comment**, type information about this command\.
   + For **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. \(Optional\) For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed instances or by specifying AWS resource groups, and you are not certain how many instances are targeted, then restrict the number of instances that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Instances still processing the command might also send errors\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an S3 bucket** box\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. In addition, if the specified S3 bucket is in a different AWS account, ensure that the instance profile associated with the instance has the necessary permissions to write to that bucket\.

1. In the **SNS notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

1. Choose **Run**\.

To see examples that use the `aws:runPowerShellScript` plugin, see [aws:runPowerShellScript](ssm-plugins.md#aws-runPowerShellScript)\.