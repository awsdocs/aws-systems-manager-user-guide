# Querying Inventory Data from Multiple Regions and Accounts<a name="systems-manager-inventory-query"></a>

AWS Systems Manager Inventory integrates with Amazon Athena to help you query inventory data from multiple AWS Regions and accounts\. Athena integration uses Resource Data Sync so that you can view inventory data from all of your managed instances on the **Inventory Detail View** page in the AWS Systems Manager console\.

**Important**  
This feature uses AWS Glue to crawl the data in your Amazon Simple Storage Service \(Amazon S3\) bucket, and Amazon Athena to query the data\. Depending on how much data is crawled and queried, you can be charged for using these services\. With AWS Glue, you pay an hourly rate, billed by the second, for crawlers \(discovering data\) and ETL jobs \(processing and loading data\)\. With Athena, you are charged based on the amount of data scanned by each query\. We encourage you to view the pricing guidelines for these services before you use Amazon Athena integration with Systems Manager Inventory\. For more information, see [Amazon Athena pricing](https://aws.amazon.com/athena/pricing/) and [AWS Glue pricing](https://aws.amazon.com/glue/pricing/)\.

You can view inventory data on the **Inventory Detail View** page in all AWS Regions where [Amazon Athena is available\.](https://docs.aws.amazon.com/general/latest/gr/rande.html#athena) 

**Before you begin**  
Athena integration uses Resource Data Sync\. You must set up and configure Resource Data Sync to use this feature\. For more information, see [Configuring Resource Data Sync for Inventory](sysman-inventory-datasync.md)\.

Also, be aware that the **Inventory Detail View** page displays inventory data for the *owner* of the central Amazon S3 bucket used by Resource Data Sync\. If you are not the owner of the central Amazon S3 bucket, then you won't see inventory data on the **Inventory Detail View** page\.

## Configuring IAM Roles and Permissions for Querying Data from Multiple Regions and Accounts<a name="systems-manager-inventory-query-iam"></a>

Before you can query and view data from multiple accounts and Regions on the **Inventory Detail View** page in the Systems Manager console, you must configure AWS Identity and Access Management \(IAM\) permissions\. More specifically, you must do the following:
+ [Task 1: Configure User Access](#systems-manager-inventory-query-access-user): Configure your IAM user account, group, or role with the **AWSQuicksightAthenaAccess** managed policy and a separate inline policy\. The inline policy provides permissions for setting up AWS Glue crawlers and enables you to view data on the **Inventory Detail View** page\.
+ [Task 2: Create the Required IAM Role](#systems-manager-inventory-query-access-role): Create a new IAM role called **Amazon\-GlueServiceRoleForSSM**\. This role enables AWS Glue to access the Resource Data Sync Amazon S3 bucket\. You must assign the **AWSGlueServiceRole** managed policy to this role\.
+ [Task 3: Create and Attach an Additional AWS Glue Policy to the IAM Role](#systems-manager-inventory-query-access-policy): Create a separate policy for the IAM role that enables communication between AWS Glue and Systems Manager Inventory\.

The following procedures describe how to configure the required roles and permissions for querying data from multiple Regions and accounts with Systems Manager Inventory\.

## Task 1: Configure User Access<a name="systems-manager-inventory-query-access-user"></a>

You must configure your IAM user account, group, or role with the **AWSQuicksightAthenaAccess** managed policy and an inline policy with permissions to set AWS Glue crawlers so that you can view inventory data on the **Inventory Detail View** page\. The following procedure describes how to add the required policies to your IAM user account\.

**To configure user access**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**, and then choose the user account you want to configure\. The **Summary** page opens\.

1. On the **Permissions** tab, choose **Add permissions**\.

1. On the **Grant permissions** page, choose **Attach existing policies directly**\.

1. In the Search field, search for **AWSQuicksightAthenaAccess**\.

1. Choose the option beside this policy, and then choose **Next: Review**\.

1. Choose **Add permissions**\.

1. Choose the user name again to return to the **Summary** page\.

1. Now add the inline policy for the Systems Manager/Athena policy\. On the **Permissions** tab, at the right side of the page, choose **Add inline policy**\. The **Create policy** page opens\.

1. Choose the **JSON** tab\.

1. Delete the existing JSON text in the editor, and then copy and paste the following policy into the JSON editor\. 

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Sid":"VisualEditor0",
            "Effect":"Allow",
            "Action":[
               "glue:GetCrawlers",
               "glue:GetCrawler",
               "glue:GetTables",
               "glue:StartCrawler",
               "glue:CreateCrawler"
            ],
            "Resource":"*"
         },
         {
            "Effect":"Allow",
            "Action":"iam:PassRole",
            "Resource":"arn:aws:iam::account_ID:role/Amazon-GlueServiceRoleForSSM"
         }
      ]
   }
   ```

1. On the **Review Policy** page, enter a name in the **Name** field\.

1. Choose **Create policy**\.

## Task 2: Create the Required IAM Role<a name="systems-manager-inventory-query-access-role"></a>

You must create a new IAM role called **Amazon\-GlueServiceRoleForSSM** that enables AWS Glue to access the Resource Data Sync Amazon S3 bucket\. You must then assign the **AWSGlueServiceRole** managed policy to this role, as described in the following procedure\.

**To create the required IAM role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Select type of trusted entity** page, under **AWS Service**, choose **EC2**\. And then choose **Next: Permissions**\.
**Note**  
If the **Select your use case** section appears, choose **EC2 Role for Simple Systems Manager**\.

1. On the **Attach permissions policies** page, use the Search field to search for **AWSGlueServiceRole**\. Choose the option beside this policy, and then choose **Next: Review**\. 

1. On the **Review** page, enter **Amazon\-GlueServiceRoleForSSM** in the **Role name** field, and then type a description\.
**Important**  
You must specify **Amazon\-GlueServiceRoleForSSM** as the name for this role\. If you don't, calls to AWS Glue from Systems Manager Inventory will fail\.

1. Choose **Create role**\. The console returns you to the **Roles** page\.

## Task 3: Create and Attach an Additional AWS Glue Policy to the IAM Role<a name="systems-manager-inventory-query-access-policy"></a>

You must add an additional IAM policy to the IAM role to enable communication between AWS Glue and Systems Manager Inventory\.

**To create and add an additional AWS Glue policy to the IAM role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. Choose the **JSON** tab\.

1. Delete the existing JSON text in the editor, and then copy and paste the following policy into the JSON editor\. 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "s3:GetObject",
                   "s3:PutObject"
               ],
               "Resource": [
                   "arn:aws:s3:::name_of_Resource_Data_Sync_S3_bucket*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogGroup",
                   "logs:CreateLogStream",
                   "logs:PutLogEvents"
               ],
               "Resource": [
                   "*"
               ]
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review Policy** page, enter a name in the **Name** field\. Optionally, you can enter a description\.

1. Choose **Create policy**\.

1. In the IAM navigation pane, choose **Roles**\.

1. Use the Search field to locate the **Amazon\-GlueServiceRoleForSSM** role\. Choose the role name\. 

1. On the **Permissions** tab, choose **Attach policies**\.

1. Use the Search field to locate the policy you just created\. 

1. Choose the option beside the policy name, and then choose **Attach policy**\.

## Querying Data on the Detailed Inventory View Page<a name="systems-manager-inventory-query-detail-view"></a>

Use the following procedure to view inventory data from multiple AWS Regions and accounts on the **Detailed Inventory View** page\.

**To view inventory data from multiple Regions and accounts in the AWS Systems Manager console**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Inventory**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Inventory** in the navigation pane\.

1. Choose the **Detailed View** tab\.  
![\[Accessing the AWS Systems Manager Inventory Detailed View page\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-detailed-view.png)

1. Choose the option next to the Resource Data Sync for which you want to query data, and then choose **Display Inventory Data**\.  
![\[Displaying Inventory data in the AWS Systems Manager console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-display-data.png)

1. In the **Inventory Type** list, choose the type of inventory data that you want to query, and then press Enter\.  
![\[Choosing an Inventory type in the AWS Systems Manager console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-type.png)

1. To filter the data, choose the Filter bar, and then choose a filter option\.  
![\[Filtering Inventory data in the AWS Systems Manager console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-filter.png)

   The following example shows AWSComponent inventory data filtered on the us\-east\-2 Region\.  
![\[Filtering Inventory data in the AWS Systems Manager console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-detailed-view-with-data.png)

You can use the **Export to CSV** button to view the current query set in a spreadsheet application such as Microsoft Excel\. You can also use the **Query History** and **Run Advanced Queries** buttons to view history details and interact with your data in Amazon Athena\.

### Editing the AWS Glue Crawler Schedule<a name="systems-manager-inventory-glue-settings"></a>

AWS Glue crawls the Systems Manager Inventory data in the central Amazon S3 bucket twice daily, by default\. If you frequently change the types of data to collect on your instances then you might want to crawl the data more frequently, as described in the following procedure\.

**Important**  
AWS Glue charges your account based on an hourly rate, billed by the second, for crawlers \(discovering data\) and ETL jobs \(processing and loading data\)\. Before you change the crawler schedule, view the [AWS Glue pricing](https://aws.amazon.com/glue/pricing/) page\.

**To change the inventory data crawler schedule**

1. Open the AWS Glue console at [https://console\.aws\.amazon\.com/glue/](https://console.aws.amazon.com/glue/)\.

1. In the navigation pane, choose **Crawlers**\.

1. In the crawlers list, choose the option beside the Systems Manager Inventory data crawler\. The crawler name uses the following format:

   AWSSystemsManager\-*Resource\_Data\_Sync\_bucket\_name*\-*Region*\-*AWS\_account\_ID*

1. Choose **Action**, and then choose **Edit crawler**\.

1. In the navigation pane, choose **Schedule**\.

1. In the **Cron expression** field, specify a new schedule by using a cron format\. For more information about the cron format, see [Time\-Based Schedules for Jobs and Crawlers](https://docs.aws.amazon.com/glue/latest/dg/monitor-data-warehouse-schedule.html) in the *AWS Glue Developer Guide*\.

**Important**  
You can pause the crawler to stop incurring charges from AWS Glue\. If you pause the crawler, or if you change the frequency so that the data is crawled less often, then the **Detailed Inventory View** might display data that is not current\.