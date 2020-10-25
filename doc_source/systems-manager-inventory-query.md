# Querying inventory data from multiple Regions and accounts<a name="systems-manager-inventory-query"></a>

AWS Systems Manager Inventory integrates with Amazon Athena to help you query inventory data from multiple AWS Regions and accounts\. Athena integration uses resource data sync so that you can view inventory data from all of your managed instances on the **Inventory Detail View** page in the AWS Systems Manager console\.

**Important**  
This feature uses AWS Glue to crawl the data in your Amazon Simple Storage Service \(Amazon S3\) bucket, and Amazon Athena to query the data\. Depending on how much data is crawled and queried, you can be charged for using these services\. With AWS Glue, you pay an hourly rate, billed by the second, for crawlers \(discovering data\) and ETL jobs \(processing and loading data\)\. With Athena, you are charged based on the amount of data scanned by each query\. We encourage you to view the pricing guidelines for these services before you use Amazon Athena integration with Systems Manager Inventory\. For more information, see [Amazon Athena pricing](https://aws.amazon.com/athena/pricing/) and [AWS Glue pricing](https://aws.amazon.com/glue/pricing/)\.

You can view inventory data on the **Inventory Detail View** page in all AWS Regions where Amazon Athena is available\. For a list of supported Regions, see [Amazon Athena Service Endpoints](https://docs.aws.amazon.com/general/latest/gr/athena.html#athena_region) in the *Amazon Web Services General Reference*\.

**Before you begin**  
Athena integration uses resource data sync\. You must set up and configure resource data sync to use this feature\. For more information, see [Configuring Resource Data Sync for Inventory](sysman-inventory-datasync.md)\.

Also, be aware that the **Inventory Detail View** page displays inventory data for the *owner* of the central S3 bucket used by resource data sync\. If you are not the owner of the central S3 bucket, then you won't see inventory data on the **Inventory Detail View** page\.

## Configuring access<a name="systems-manager-inventory-query-iam"></a>

Before you can query and view data from multiple accounts and Regions on the **Inventory Detail View** page in the Systems Manager console, you must configure your AWS Identity and Access Management \(IAM\) user account\. The following procedure describes how to use the IAM console to configure your IAM user account so that you can view inventory data on the **Inventory Detail View** page\. 

**To configure access to the inventory detail view page**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**, and then choose the user account you want to configure\. The **Summary** page opens\.

1. On the **Permissions** tab, choose **Add permissions**\.

1. On the **Grant permissions** page, choose **Attach existing policies directly**\.

1. In the Search field, search for **AWSQuicksightAthenaAccess**\.

1. Choose the option next to this policy, and then choose **Next: Review**\.

1. Choose **Add permissions**\.

1. Choose the user name again to return to the **Summary** page\.

1. Now add an inline policy so that AWS Glue can crawl your inventory data\. On the **Permissions** tab, at the right side of the page, choose **Add inline policy**\. The **Create policy** page opens\.

1. Choose the **JSON** tab\.

1. Delete the existing JSON text in the editor, and then copy and paste the following policy into the JSON editor\. 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "glue:GetCrawlers",
                   "glue:GetCrawler",
                   "glue:GetTables",
                   "glue:StartCrawler",
                   "glue:CreateCrawler"
               ],
               "Resource": "*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "iam:PassRole",
                   "iam:CreateRole",
                   "iam:AttachRolePolicy"
               ],
               "Resource": [
                   "arn:aws:iam::account_ID:role/*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "iam:CreatePolicy"
               ],
               "Resource": [
                   "arn:aws:iam::account_ID:policy/*"
               ]
           }
       ]
   }
   ```
**Note**  
\(Optional\) If the Amazon Simple Storage Service \(Amazon S3\) bucket used to store inventory data is encrypted by using the AWS Key Management Service, then you must also add the following block to the policy\.  

   ```
   {
       "Effect": "Allow",
       "Action": [
           "kms:Decrypt"
       ],
       "Resource": [
           "arn:aws:kms:AWS_Region:account_ID:key/key_ARN"
       ]
   }
   ```
If you paste this block after the last block in the policy, be sure to separate the blocks with a comma \(,\)\.

1. On the **Review Policy** page, enter a name in the **Name** field\.

1. Choose **Create policy**\.

**Important**  
When you choose a resource data sync on the **Inventory Detail View** page, Systems Manager automatically creates the **Amazon\-GlueServiceRoleForSSM** role\. This role enables AWS Glue to access the S3 bucket for resource data sync\. Systems Manager automatically attaches the following policies to the role:  
**Amazon\-GlueServicePolicyForSSM\-\{*Amazon S3 bucket name*\}**: This policy enables communication between AWS Glue and Systems Manager Inventory\.
**AWSGlueServiceRole**: This is an AWS managed policy that enables access to AWS Glue\.
If a policy with the name **Amazon\-GlueServicePolicyForSSM\-\{*S3 bucket name*\}** already exists in your IAM user account, and this policy is not attached to the **Amazon\-GlueServiceRoleForSSM** role, then the system returns an error\. To resolve this issue, use the IAM console to verify that the contents of the **Amazon\-GlueServicePolicyForSSM\-\{*Amazon S3 bucket name*\}** policy match the inline policy in this procedure\. Then attach the policy to the **Amazon\-GlueServiceRoleForSSM** role\.

## Querying data on the inventory detailed view page<a name="systems-manager-inventory-query-detail-view"></a>

Use the following procedure to view inventory data from multiple AWS Regions and accounts on the Systems Manager Inventory **Detailed View** page\.

**Important**  
The Inventory **Detailed View** page is only available in AWS Regions that offer Amazon Athena\. If the following tabs are not displayed on the Systems Manager Inventory page, it means Athena is not available in the Region and you can't use the **Detailed View** to query data\.  

![\[Displaying Inventory Dashboard | Detailed View | Settings tabs\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-detailed-view-for-error.png)

**To view inventory data from multiple Regions and accounts in the AWS Systems Manager console**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Inventory**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Inventory** in the navigation pane\.

1. Choose the **Detailed View** tab\.  
![\[Accessing the AWS Systems Manager Inventory Detailed View page\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-detailed-view.png)

1. Choose the resource data sync for which you want to query data\.  
![\[Displaying inventory data in the AWS Systems Manager console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-display-data.png)

1. In the **Inventory Type** list, choose the type of inventory data that you want to query, and then press Enter\.  
![\[Choosing an inventory type in the AWS Systems Manager console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-type.png)

1. To filter the data, choose the Filter bar, and then choose a filter option\.  
![\[Filtering inventory data in the AWS Systems Manager console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-filter.png)

   The following example shows AWSComponent inventory data filtered on the us\-east\-2 Region\.  
![\[Filtering inventory data in the AWS Systems Manager console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-detailed-view-with-data.png)

You can use the **Export to CSV** button to view the current query set in a spreadsheet application such as Microsoft Excel\. You can also use the **Query History** and **Run Advanced Queries** buttons to view history details and interact with your data in Amazon Athena\.

### Editing the AWS Glue crawler schedule<a name="systems-manager-inventory-glue-settings"></a>

AWS Glue crawls the inventory data in the central S3 bucket twice daily, by default\. If you frequently change the types of data to collect on your instances then you might want to crawl the data more frequently, as described in the following procedure\.

**Important**  
AWS Glue charges your account based on an hourly rate, billed by the second, for crawlers \(discovering data\) and ETL jobs \(processing and loading data\)\. Before you change the crawler schedule, view the [AWS Glue pricing](https://aws.amazon.com/glue/pricing/) page\.

**To change the inventory data crawler schedule**

1. Open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, choose **Crawlers**\.

1. In the crawlers list, choose the option next to the Systems Manager Inventory data crawler\. The crawler name uses the following format:

   AWSSystemsManager\-*DOC\-EXAMPLE\-BUCKET*\-*Region*\-*AWS\_account\_ID*

1. Choose **Action**, and then choose **Edit crawler**\.

1. In the navigation pane, choose **Schedule**\.

1. In the **Cron expression** field, specify a new schedule by using a cron format\. For more information about the cron format, see [Time\-Based Schedules for Jobs and Crawlers](https://docs.aws.amazon.com/glue/latest/dg/monitor-data-warehouse-schedule.html) in the *AWS Glue Developer Guide*\.

**Important**  
You can pause the crawler to stop incurring charges from AWS Glue\. If you pause the crawler, or if you change the frequency so that the data is crawled less often, then the Inventory **Detailed View** might display data that is not current\.