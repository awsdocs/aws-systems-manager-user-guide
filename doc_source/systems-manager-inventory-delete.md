# Stopping data collection and deleting inventory data<a name="systems-manager-inventory-delete"></a>

If you no longer want to use AWS Systems Manager Inventory to view metadata about your AWS resources, you can stop data collection and delete data that has already been collected\. This section includes the following information\.

**Topics**
+ [Stopping data collection](#systems-manager-inventory-delete-association)
+ [Deleting an Inventory resource data sync](#systems-manager-inventory-delete-RDS)

## Stopping data collection<a name="systems-manager-inventory-delete-association"></a>

When you initially configure Systems Manager to collect inventory data, the system creates a State Manager association that defines the schedule and the resources from which to collect metadata\. You can stop data collection by deleting any State Manager associations that use the `AWS-GatherSoftwareInventory` document\.

**To delete an Inventory association**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose ** State Manager**\.

1. Choose an association that uses the `AWS-GatherSoftwareInventory` document and then choose **Delete**\.

1. Repeat step three for any remaining associations that use the `AWS-GatherSoftwareInventory` document\.

## Deleting an Inventory resource data sync<a name="systems-manager-inventory-delete-RDS"></a>

If you no longer want to use AWS Systems Manager Inventory to view metadata about your AWS resources, then we also recommend deleting resource data syncs used for inventory data collection\.

**To delete an Inventory resource data sync**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Inventory**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Inventory** in the navigation pane\.

1. Choose **Resource Data Syncs**\.

1. Choose a sync in the list\.
**Important**  
Make sure you choose the sync used for Inventory\. Systems Manager supports resource data sync for multiple capabilities\. If you choose the wrong sync, you could disrupt data aggregation for Systems Manager Explorer or Systems Manager Compliance\.

1. Choose **Delete**

1. Repeat these steps for any remaining resource data syncs you want to delete\.

1. Delete the Amazon Simple Storage Service \(Amazon S3\) bucket where the data was stored\. For information about deleting an Amazon S3 bucket, see [Deleting a bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/delete-bucket.html)\.