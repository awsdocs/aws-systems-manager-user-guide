# Deregistering managed instances in a hybrid environment<a name="systems-manager-managed-instances-advanced-deregister"></a>

If you no longer want to manage an on\-premises server or virtual machine \(VM\) by using AWS Systems Manager, then you can deregister it\. Deregistering a hybrid machine removes it from the list of managed instances in Systems Manager\. SSM Agent running on the hybrid machine won't be able to refresh its authorization token because it's no longer registered\. SSM Agent will hibernate and reduce its ping frequency to Systems Manager in the cloud to once per hour\.

You can reregister an on\-premises server or VM again at any time\. Systems Manager stores the command history for a deregistered managed instance for 30 days\.

The following procedure describes how to deregister a hybrid machine by using the AWS Systems Manager console\. For information about how to do this by using the AWS CLI, see [deregister\-managed\-instance](https://docs.aws.amazon.com/cli/latest/reference/ssm/deregister-managed-instance.html)\.

**To deregister a hybrid machine \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed instances**\.

1. In the **Managed instances** list, choose the instance you want to deregister and then choose **Actions**, **Deregister this managed instance**\.

1. Review the information in the **Deregister this managed instance** pop\-up, and then if you approve, choose **Deregister**\.