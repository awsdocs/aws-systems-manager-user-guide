# Setting up Systems Manager Explorer to display data from multiple accounts and Regions<a name="Explorer-resource-data-sync"></a>

AWS Systems Manager uses an integrated setup experience to help you get started with AWS Systems Manager Explorer *and* AWS Systems Manager OpsCenter\. After completing Integrated Setup, Explorer and OpsCenter automatically synchronize data\. More specifically, these capabilities synchronize OpsData and OpsItems for the Amazon Web Services account and AWS Region you used when you completed Integrated Setup\. If you want to aggregate OpsData and OpsItems from other accounts and Regions, you must create a resource data sync, as described in this topic\.

**Note**  
For more information about Integrated Setup, see [Getting started with Systems Manager Explorer and OpsCenter](Explorer-setup.md)\.

**About resource data sync for Explorer**  
Resource data sync for Explorer offers two aggregation options:
+ **Single\-account/Multiple\-regions:** You can configure Explorer to aggregate OpsItems and OpsData data from multiple AWS Regions, but the data set is limited to the current Amazon Web Services account\.
+ **Multiple\-accounts/Multiple\-regions:** You can configure Explorer to aggregate data from multiple AWS Regions and accounts\. This option requires that you set up and configure AWS Organizations\. After you set up and configure AWS Organizations, you can aggregate data in Explorer by organizational unit \(OU\) or for an entire organization\. Systems Manager aggregates the data into the AWS Organizations management account before displaying it in Explorer\. For more information, see [What is AWS Organizations?](https://docs.aws.amazon.com/organizations/latest/userguide/) in the *AWS Organizations User Guide*\.

The following diagram shows a resource data sync configured to work with AWS Organizations\. In this scenario, the user has two accounts defined in AWS Organizations\. Resource data sync aggregates data from both accounts and multiple AWS Regions into the AWS Organizations management account where it is then displayed in Explorer\.

![\[Resource data sync for Systems Manager Explorer\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/ExplorerSyncFromSource.png)

## About multiple account and Region resource data syncs<a name="Explorer-resource-data-sync-multiple-accounts-and-regions"></a>

This section describes important details about multiple account and multiple Region resource data syncs that use AWS Organizations\. Specifically, the information in this section applies if you choose one of the following options in the **Create resource data sync** page:
+ Include all accounts from my AWS Organizations configuration
+ Select organization units in AWS Organizations

If you don't plan to use one of these options, you can skip this section\.

When you create a resource data sync, if you choose one of the AWS Organizations options, then Systems Manager automatically enables all OpsData sources in the selected Regions for all Amazon Web Services accounts in your organization \(or in the selected organizational units\)\. For example, even if you haven't enabled Explorer in a Region, if you select an AWS Organizations option for your resource data sync, then Systems Manager automatically collects OpsData from that Region\. 

If you don't choose one of the AWS Organizations options for a resource data sync, then you must complete Integrated Setup in each account and Region where you want Explorer to access data\. If you don't, Explorer won't display OpsData and OpsItems for those accounts and Regions in which you didn't complete Integrated Setup\.

If you add a child account to your organization, Explorer automatically enables all OpsData sources for the account\. If, at a later time, you remove the child account from your organization, Explorer continues to collect OpsData from the account\. 

If you update an existing resource data sync that uses one of the AWS Organizations options, the system prompts you to approve collection of all OpsData sources for all accounts and Regions affected by the change\.

If you add a new service to your Amazon Web Services account, and if Explorer collects OpsData for that service, Systems Manager automatically configures Explorer to collect that OpsData\. For example, if your organization did not use AWS Trusted Advisor when you previously created a resource data sync, but your organization signs up for this service, Explorer automatically updates your resource data syncs to collect this OpsData\.

**Important**  
Note the following important information about multiple account and Region resource data syncs:  
Deleting a resource data sync does not disable an OpsData source in Explorer\. 
To view OpsData and OpsItems from multiple accounts, you must have the AWS Organizations **All features** mode enabled and you must be signed into the AWS Organizations management account\.

## Creating a resource data sync<a name="Explorer-resource-data-sync-configuring-multi"></a>

Before you configure resource data sync for Explorer, note the following details\.
+ Explorer supports a maximum of five resource data syncs\.
+ After you create a resource data sync for a Region, you can't change the *account options* for that sync\. For example, if you create a sync in the us\-east\-2 \(Ohio\) Region and you choose the **Include only the current account** option, you can't edit that sync later and choose the **Include all accounts from my AWS Organizations configuration** option\. Instead, you must delete the first resource data sync, and create a new one\. For more information, see [Deleting a Systems Manager Explorer resource data sync](Explorer-using-resource-data-sync-delete.md)
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
To use either of the AWS Organizations options, you must be logged into the AWS Organizations management account or you must be logged into an Explorer delegated administrator account\. For more information about the delegated administrator account, see [Configuring a Delegated Administrator](Explorer-setup-delegated-administrator.md)\.

1. In the **Regions to include** section, choose one of the following options\.
   + Choose **All current and future regions** to automatically sync data from all current AWS Regions and any new Regions that come online in the future\.
   + Choose **All regions** to automatically sync data from all current AWS Regions\.
   + Individually choose Regions that you want to include\.

1. Choose **Create resource data sync**\.

The system can take several minutes to populate Explorer with data after you create a resource data sync\. You can view the sync by choosing it from the **Select a resource data sync** list in Explorer\.