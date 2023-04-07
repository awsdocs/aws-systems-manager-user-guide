# Deleting associations<a name="systems-manager-state-manager-delete-association"></a>

The following procedures describe how to delete State Manager associations by using the AWS Systems Manager console\.

## Delete an association<a name="systems-manager-state-manager-delete-an-association"></a>

 Use the following procedure to delete an association by using the AWS Systems Manager console\.

**To delete an association**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose ** State Manager**\.

1. Select an association and then choose **Delete**\.

## Delete multiple associations<a name="systems-manager-state-manager-delete-associations-bulk"></a>

You can delete multiple associations in a single operation by running an automation from the AWS Systems Manager console\. When you select multiple associations for deletion, State Manager launches the automation runbook start page with the association IDs entered as input parameter values\. 



**To delete multiple associations in a single operation**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose ** State Manager**\.

1. Select each association that you want to delete and then choose **Delete**\.

1. \(Optional\) In the **Additional input parameters** area, select the Amazon Resource Name \(ARN\) for the *assume role* that you want the automation to use while running\. To create a new assume role, choose **Create**\.

1. Choose **Submit**\.