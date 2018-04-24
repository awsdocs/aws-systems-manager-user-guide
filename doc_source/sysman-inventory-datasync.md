# Configuring Resource Data Sync for Inventory<a name="sysman-inventory-datasync"></a>

You can use Systems Manager Resource Data Sync to send Inventory data collected from all of your managed instances to a single Amazon S3 bucket\. Resource Data Sync then automatically updates the centralized data when new Inventory data is collected\. With all Inventory data stored in a target Amazon S3 bucket, you can use services like Amazon Athena and Amazon QuickSight to query and analyze the aggregated data\.

For example, say that you've configured Inventory to collect data about the operating system \(OS\) and applications running on a fleet of 150 managed instances\. Some of these instances are located in a hybrid data center, and others are running in Amazon EC2 across multiple AWS Regions\. If you have *not* configured Resource Data Sync for Inventory, you either need to manually gather the collected inventory data for each instance, or you have to create scripts to gather this information\. You would then need to port the data into an application so that you can run queries and analyze it\.

With Resource Data Sync, you perform a one\-time operation that synchronizes all Inventory data from all of your managed instances\. When you create the sync, you can specify managed instances from multiple AWS accounts and AWS Regions\. After the sync is successfully created, Systems Manager creates a baseline of all Inventory data and saves it in the target Amazon S3 bucket\. When new inventory data is collected, Systems Manager automatically updates the data in the Amazon S3 bucket\. You can then quickly and cost\-effectively port the data to Amazon Athena and Amazon QuickSight\.

Diagram 1 shows how Resource Data Sync aggregates inventory data from managed instances in Amazon EC2 and a hybrid environment to a target Amazon S3 bucket\. This diagram also shows how Resource Data Sync works with multiple AWS accounts and AWS Regions\.

**Diagram 1: Resource Data Sync with Multiple AWS Accounts and AWS Regions**

![\[Systems Manager Resource Data Sync architecture\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/ssm-inventory-datasync-vsd.png)

If you delete a managed instance, Resource Data Sync preserves the Inventory file for the deleted instance\. For running instances, however, Resource Data Sync automatically overwrites old inventory files when new files are created and written to the Amazon S3 bucket\. If you want to track inventory changes over time, you can use the AWS Config service to track the MangagedInstanceInventory resource type\. For more information, see [Getting Started with AWS Config](http://docs.aws.amazon.com/config/latest/developerguide/getting-started.html)\.

**Related Content**
+ Resource Data Sync uses the following API actions: [CreateResourceDataSync](http://docs.aws.amazon.com/ssm/latest/APIReference/API_CreateResourceDataSync.html), [ListResourceDataSync](http://docs.aws.amazon.com/ssm/latest/APIReference/API_ListResourceDataSync.html), and [DeleteResourceDataSync](http://docs.aws.amazon.com/ssm/latest/APIReference/API_DeleteResourceDataSync.html)\.
+ Amazon QuickSight is a business analytics service that makes it easy to build visualizations so that you can analyze and gather insights from your data\. Connecting to Athena from QuickSight is a one\-click process\. You don't need to provide endpoints or a user name and password\. You can simply choose Athena as your data source, choose the database and tables to analyze, and start visualizing the data in QuickSight\. For more information, see [Amazon QuickSight User Guide](http://docs.aws.amazon.com/quicksight/latest/user/)\. 
+ Amazon Athena is an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL queries\. Athena doesn't require an Amazon EC2 instance to run, so there is no infrastructure to manage\. You pay only for the queries that you run\. For more information, see [Amazon Athena User Guide](http://docs.aws.amazon.com/athena/latest/ug/)\.

**Topics**
+ [Create a Resource Data Sync for Inventory](sysman-inventory-datasync-create.md)