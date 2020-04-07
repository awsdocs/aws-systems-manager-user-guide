# Deleting a Systems Manager Explorer Resource Data Sync<a name="Explorer-using-resource-data-sync-delete"></a>

In Systems Manager Explorer, you can aggregate OpsData and OpsItems from other accounts and Regions by creating a resource data sync\.

You can't change the account options for a resource data sync\. For example, if you created a sync in the us\-east\-2 \(Ohio\) Region and you chose the **Include only the current account** option, you can't edit that sync later and choose the **Include all accounts from my AWS Organizations configuration** option\. Instead, you must delete the resource data sync, and create a new one, as described in the following procedure\.

**To delete a resource data sync**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Choose **Settings**\.

1. In the **Configure resource data sync** section, choose the resource data sync that you want to delete\.

1. Choose **Delete**\.