# Deleting a resource data sync for Compliance<a name="systems-manager-compliance-delete-RDS"></a>

If you no longer want to use AWS Systems Manager Compliance to view compliance data, then we also recommend deleting resource data syncs used for Compliance data collection\.

**To delete a Compliance resource data sync**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose **Account management**, **Resource data syncs**\.

1. Choose a sync in the list\. 
**Important**  
Make sure you choose the sync used for Compliance\. Systems Manager supports resource data sync for multiple capabilities\. If you choose the wrong sync, you could disrupt data aggregation for Systems Manager Explorer or Systems Manager Inventory\.

1. Choose **Delete**\.

1. Delete the Amazon Simple Storage Service \(Amazon S3\) bucket where the data was stored\. For information about deleting an Amazon S3 bucket, see [Deleting a bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/delete-bucket.html)\.