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

## Creating a Resource Data Sync for Inventory<a name="sysman-inventory-datasync-create"></a>

Use the following procedure to create a Resource Data Sync for Inventory by using the Amazon EC2 console\. 

**Note**  
You can use AWS Key Management Service \(AWS Key Management Service\) to encrypt the data sent by Systems Manager to the centralized Amazon S3 bucket\. Currently, you can only configure encryption by using the AWS CLI\. For an example of how to create an encrypted sync by using the AWS CLI and how to work with the centralized data in Amazon Athena and Amazon QuickSight, see [Using Resource Data Sync to Aggregate Inventory Data](sysman-inventory-walk.md#sysman-inventory-resource-data-sync)\. 

**To create and configure an Amazon S3 Bucket for Resource Data Sync**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Create a bucket to store your aggregated Inventory data\. For more information, see [Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the *Amazon Simple Storage Service Getting Started Guide*\. Make a note of the bucket name and the AWS Region where you created it\.

1. Choose the **Permissions** tab, and then choose **Bucket Policy**\.

1. Copy and paste the following bucket policy into the policy editor\. Replace *Bucket\-Name* and *Account\-ID* with the name of the Amazon S3 bucket you created and a valid AWS account ID\. Optionally, replace *Bucket\-Prefix* with the name of an Amazon S3 prefix \(subdirectory\)\. If you didn't create a prefix, remove *Bucket\-Prefix*/ from the ARN in the policy\. 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "SSMBucketPermissionsCheck",
               "Effect": "Allow",
               "Principal": {
                   "Service": "ssm.amazonaws.com"
               },
               "Action": "s3:GetBucketAcl",
               "Resource": "arn:aws:s3:::Bucket-Name"
           },
           {
               "Sid": " SSMBucketDelivery",
               "Effect": "Allow",
               "Principal": {
                   "Service": "ssm.amazonaws.com"
               },
               "Action": "s3:PutObject",
               "Resource": ["arn:aws:s3:::Bucket-Name/Bucket-Prefix/*/accountid=Account_ID_number/*"],
               "Condition": {
                   "StringEquals": {
                       "s3:x-amz-acl": "bucket-owner-full-control"
                   }
               }
           }
       ]
   }
   ```

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create a Resource Data Sync \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed Instances**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Managed Instances**\.

1. Choose **Resource Data Syncs**, and then choose **Create resource data sync**\.

1. In the **Sync name** field, type a name for the sync configuration\.

1. In the **Bucket name** field, type the name of the Amazon S3 bucket you created at the start of this procedure\.

1. \(Optional\) In the **Bucket prefix** field, type the name of an Amazon S3 bucket prefix \(subdirectory\)\.

1. In the **Bucket region** field, choose **This region** if the Amazon S3 bucket you created is located in the current AWS Region\. If the bucket is located in a different AWS Region, choose **Another region**, and type the name of the Region\.
**Note**  
If the sync and the target Amazon S3 bucket are located in different regions, you may be subject to data transfer pricing\. For more information, see [Amazon S3 Pricing](https://aws.amazon.com//s3/pricing/)\.

1. Choose **Create**\.

**To create a Resource Data Sync \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Shared Resources** in the navigation pane, and choose **Managed Instances**\.

1. Choose **Resource Data Syncs**, and then choose **Create a Resource Data Sync**\.

1. In the **Sync Name** field, type a name for the sync configuration\.

1. In the **Bucket Name** field, type the name of the Amazon S3 bucket you created at the start of this procedure\.

1. \(Optional\) In the **Bucket Prefix** field, type the name of an Amazon S3 bucket prefix \(subdirectory\)\.

1. In the **Bucket Region** field, choose **This region** if the Amazon S3 bucket you created is located in the current AWS Region\. If the bucket is located in a different AWS Region, choose **Another region**, and type the name of the Region\.
**Note**  
If the sync and the target Amazon S3 bucket are located in different regions, you may be subject to data transfer pricing\. For more information, see [Amazon S3 Pricing](https://aws.amazon.com//s3/pricing/)\.

1. Choose **Create**\.