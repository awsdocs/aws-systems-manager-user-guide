# Setting up Systems Manager Explorer to display data from multiple accounts and Regions<a name="Explorer-resource-data-sync"></a>

Systems Manager uses an integrated setup experience to help you get started with Systems Manager Explorer *and* Systems Manager OpsCenter\. After completing Integrated Setup, Explorer and OpsCenter automatically synchronize data\. More specifically, these capabilities synchronize OpsData and OpsItems for the AWS account and Region you used when you completed Integrated Setup\. If you want to aggregate OpsData and OpsItems from other accounts and Regions, you must create a resource data sync, as described in this topic\.

**Note**  
For more information about Integrated Setup, see [Getting started with Systems Manager Explorer and OpsCenter](Explorer-setup.md)\.

**About Resource Data Sync for Explorer**  
Resource data sync for Explorer offers two aggregation options:
+ **Single\-account/Multiple\-regions:** You can configure Explorer to aggregate OpsItems and OpsData data from multiple AWS Regions, but the data set is limited to the current AWS account\.
+ **Multiple\-accounts/Multiple\-regions:** You can configure Explorer to aggregate data from multiple AWS Regions and accounts\. This option requires that you set up and configure AWS Organizations\. After you set up and configure AWS Organizations, you can aggregate data in Explorer by organizational unit \(OU\) or for an entire organization\. Systems Manager aggregates the data into the AWS Organizations master account before displaying it in Explorer\. For more information, see [What is AWS Organizations?](https://docs.aws.amazon.com/organizations/latest/userguide/) in the *AWS Organizations User Guide*\.

The following diagram shows a resource data sync configured to work with AWS Organizations\. In this scenario, the user has two accounts defined in AWS Organizations\. Resource data sync aggregates data from both accounts and multiple AWS Regions into the AWS Organizations master account where it is then displayed in Explorer\.

![\[Resource data sync for Systems Manager Explorer\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/ExplorerSyncFromSource.png)

**Before You Begin**  
Note the following important details about resource data sync for Explorer\.
+ Explorer supports a maximum of five resource data syncs\.
+ You must complete Integrated Setup in each account and Region where you want Explorer to access data\. If you don't, Explorer won't display OpsData and OpsItems for those accounts and Regions in which you didn't complete Integrated Setup\.
+ To view OpsData and OpsItems from multiple accounts, you must have the AWS Organizations **All features** mode enabled and you must be signed into the AWS Organizations Master Account\.
+ After you create a resource data sync for a Region, you can't change the account options for that sync\. For example, if you create a sync in the us\-east\-2 \(Ohio\) Region and you choose the **Include only the current account** option, you can't edit that sync later and choose the **Include all accounts from my AWS Organizations configuration** option\. Instead, you must delete the first resource data sync, and create a new one\. For more information, see [Deleting a Systems Manager Explorer Resource Data Sync](Explorer-using-resource-data-sync-delete.md)
+ OpsData viewed in Explorer is read\-only\.

Use the following procedure to create a resource data sync for Explorer\.

**To create a resource data sync**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Choose **Settings**\.

1. In the **Configure resource data sync** section, choose **Create resource data sync**\.

1. For **Resource data sync name**, enter a name\.

1. In the **Add accounts** section, choose an option\.
**Note**  
To use either of the AWS Organizations options, you must be logged into the AWS Organizations master account or you must be logged into an Explorer delegated administrator account\. For more information about the delegated administrator account, see [Configuring a Delegated Administrator](Explorer-setup-delegated-administrator.md)\.

1. In the **Regions to include** section, choose one of the following options\.
   + Choose **All current and future regions** to automatically sync data from all current AWS Regions and any new Regions that come online in the future\.
   + Choose **All regions** to automatically sync data from all current AWS Regions\.
   + Individually choose Regions that you want to include\.

1. Choose **Create resource data sync**\.

The system can take several minutes to populate Explorer with data after you create a resource data sync\. You can view the sync by choosing it from the **Select a resource data sync** list in Explorer\.