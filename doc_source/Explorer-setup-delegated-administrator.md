# Configuring a Delegated Administrator<a name="Explorer-setup-delegated-administrator"></a>

If you aggregate Explorer data from multiple AWS Regions and accounts by using resource data sync with AWS Organizations, then we suggest that you configure a delegated administrator for Explorer\. A delegated administrator improves Explorer security in the following ways\.
+ You limit the number of Explorer administrators who can create or delete multi\-account and Region resource data syncs to only one individual\.
+ You no longer need to be logged into the AWS Organizations master account to administer resource data syncs in Explorer\.

For more information about resource data sync, see [Setting up Systems Manager Explorer to display data from multiple accounts and Regions](Explorer-resource-data-sync.md)\. For more information about AWS Organizations, see [What is AWS Organizations?](https://docs.aws.amazon.com/organizations/latest/userguide/) in the *AWS Organizations User Guide*\.

**Topics**
+ [Before you begin](#Explorer-setup-delegated-administrator-before-you-begin)
+ [Configure an Explorer delegated administrator](#Explorer-setup-delegated-administrator-configure)
+ [Deregister an Explorer delegated administrator](#Explorer-setup-delegated-administrator-deregister)

## Before you begin<a name="Explorer-setup-delegated-administrator-before-you-begin"></a>

The following list includes important information about Explorer delegated administration\.
+ You can delegate only one account for Explorer administration\.
+ The account ID that you specify as an Explorer delegated administrator must be listed as a member account in AWS Organizations\. For more information, see [Creating an AWS account in your organization](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts_create.html) in the *AWS Organizations User Guide*\.
+ A delegated adminstator can use all Explorer resource data sync API actions in the console or by using programmatic tools such as the SDK, the AWS CLI, or AWS Tools for Windows PowerShell\. Resource data sync API actions include the following: [CreateResourceDataSync](https://docs.aws.amazon.com/ssm/latest/APIReference/API_CreateResourceDataSync.html), [DeleteResourceDataSync](https://docs.aws.amazon.com/ssm/latest/APIReference/API_DeleteResourceDataSync.html), [ListResourceDataSync](https://docs.aws.amazon.com/ssm/latest/APIReference/API_ListResourceDataSync.html), and [UpdateResourceDataSync](https://docs.aws.amazon.com/ssm/latest/APIReference/API_UpdateResourceDataSync.html)\.
+ A delegated administrator can search, filter, and aggregate Explorer data in the console or by using programmatic tools such as the SDK, the AWS CLI, or AWS Tools for Windows PowerShell\. Search, filter, and data aggregation use the [GetOpsSummary](https://docs.aws.amazon.com/ssm/latest/APIReference/API_GetOpsSummary.html) API action\.
+ Resource data syncs created by a delegated administrator are only available in the delegated administrator account\. You can't view the syncs or the aggregated data in the AWS Organizations master account\.
+ A delegated administrator can create a maximum of five resource data syncs\.
+ A delegated administrator can create a resource data sync for either an entire organization in AWS Organizations or a subset of organizational units\.

## Configure an Explorer delegated administrator<a name="Explorer-setup-delegated-administrator-configure"></a>

Use the following procedure to register an Explorer delegated administrator\.

**To register an Explorer delegated administrator**

1. Log into your AWS Organizations master account\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Choose **Settings**\.

1. In the **Delegated administrator for Explorer** section, verify that you have configured the required service\-linked role and service access options\. If necessary, choose the buttons to configure these options\.  
![\[Delegated adminstration section in Explorer Settings page.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/explorer-delegated-administration-1.png)

1. For **Account ID**, enter the AWS account ID\. This account must be a member account in AWS Organizations\.

1. Choose **Register delegated administrator**\.

The delegated administrator now has access to the following AWS Organizations options on the **Create resource data sync** page\. 

![\[AWS Organizations options for Explorer resource data sync.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/explorer-delegated-administration-2.png)

## Deregister an Explorer delegated administrator<a name="Explorer-setup-delegated-administrator-deregister"></a>

Use the following procedure to deregister an Explorer delegated administrator\. A delegated administrator account can only be deregistered by the AWS Organizations master account\. When a delegated administrator account is deregistered, the system deletes all AWS Organizations resource data syncs created by the delegated administrator\.

**To deregister an Explorer delegated administrator**

1. Log into your AWS Organizations master account\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Choose **Settings**\.

1. In the **Delegated administrator for Explorer** section, choose **Deregister**\. The system displays a warning\.

1. Type the account ID and choose **Remove**\.

The account no longer has access to the AWS Organizations resource data sync API actions\. The system deletes all AWS Organizations resource data syncs created by the account\.