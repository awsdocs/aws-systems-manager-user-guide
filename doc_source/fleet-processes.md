# Working with processes<a name="fleet-processes"></a>

You can use Fleet Manager, a capability of AWS Systems Manager, to work with processes on your managed instances\. Using Fleet Manager, you can view information about processes\. For example, you can see the CPU utilization and memory usage of processes in addition to their handles and threads\. With Fleet Manager, you can start and terminate processes from the console\.

**Note**  
Fleet Manager uses Session Manager, a capability of AWS Systems Manager, to retrieve process data\. For Amazon Elastic Compute Cloud \(Amazon EC2\) instances, the instance profile attached to your managed instances must provide permissions for Session Manager to use this feature\. For more information about adding Session Manager permissions to an instance profile, see [Adding Session Manager permissions to an existing IAM role](getting-started-add-permissions-to-existing-profile.md)\. Also, AWS Key Management Service \(AWS KMS\) encryption must be turned on in your session preferences to use Fleet Manager features\. For more information about turning on AWS KMS encryption for Session Manager, see [Turn on KMS key encryption of session data \(console\)](session-preferences-enable-encryption.md)\.

**To view details about processes with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Select the link of the instance whose processes you want to view\.

1. In the **Tools** menu, choose **Processes**\.

**To start a process with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Select the link of the instance you want to start a process on\.

1. In the **Tools** menu, choose **Processes**\.

1. Select **Start new process**\.

1. In the **Process name or full path** field, enter the name of the process or the full path to the executable\.

1. \(Optional\) In the **Working directory** field, enter the directory path where you want the process to run\.

**To terminate a process with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Select the link of the instance you want to start a process on\.

1. In the **Tools** menu, choose **Processes**\.

1. Choose the button next to the process you want to terminate\.

1. In the **Actions** dropdown, choose **Terminate process** or **Terminate process tree**\. 
**Note**  
Terminating a process tree also terminates all processes and applications using that process\.