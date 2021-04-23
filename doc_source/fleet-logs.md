# View logs<a name="fleet-logs"></a>

You can use Fleet Manager, a capability of AWS Systems Manager, to view log data stored on your instances\. For Windows instances, you can view Windows event logs and copy their details from the console\. To help you search events, filter Windows event logs by **Event level**, **Event ID**, **Event source**, and **Time created**\. You can also view other log data using the procedure to view the file system\. For more information about viewing the file system with Fleet Manager, see [Viewing the file system ](fleet-file-management.md)\.

**To view Windows event logs with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance whose event logs you want to view\.

1. Choose **View details**\.

1. In the **Tools** menu, choose **Windows event logs**\.

1. Choose the **Log name** that contains the events you want to view\.

1. Choose the button next to the **Log name** you want to view, and then select **View events**\.

1. Choose the button next to the event you want to view, and then select **View event details**\.

1. \(Optional\) Select **Copy as JSON** to copy the event details to your clipboard\.