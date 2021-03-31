# Deregistering managed instances in a hybrid environment<a name="systems-manager-managed-instances-advanced-deregister"></a>


|  | 
| --- |
| Systems Manager Managed Instances is now part of Systems Manager Fleet Manager\. To learn more about Fleet Manager, see [AWS Systems Manager Fleet Manager](fleet.md)\. | 

If you no longer want to manage an on\-premises server or virtual machine \(VM\) by using AWS Systems Manager, then you can deregister it\. Deregistering a hybrid machine removes it from the list of managed instances in Systems Manager\. AWS Systems Manager SSM Agent \(SSM Agent\) running on the hybrid machine won't be able to refresh its authorization token because it's no longer registered\. SSM Agent will hibernate and reduce its ping frequency to Systems Manager in the cloud to once per hour\.

You can reregister an on\-premises server or VM again at any time\. Systems Manager stores the command history for a deregistered managed instance for 30 days\.

The following procedure describes how to deregister a hybrid machine by using the Systems Manager console\. For information about how to do this by using the AWS Command Line Interface \(AWS CLI\), see [deregister\-managed\-instance](https://docs.aws.amazon.com/cli/latest/reference/ssm/deregister-managed-instance.html)\.

**To deregister a hybrid machine \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance that you want to deregister\.

1. In the **Instance actions** menu, choose **Deregister this managed instance**\.

1. Review the information in the **Deregister this managed instance** pop\-up, and then if you approve, choose **Deregister**\.