# Configuring a delegated administrator<a name="Explorer-setup-delegated-administrator"></a>

If you aggregate AWS Systems Manager Explorer data from multiple AWS Regions and accounts by using resource data sync with AWS Organizations, then we recommend that you configure a delegated administrator for Explorer\. 

A delegated administrator can use the following Explorer resource data sync APIs using the console, SDK, AWS Command Line Interface \(AWS CLI\), or AWS Tools for Windows PowerShell: 
+ [CreateResourceDataSync](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateResourceDataSync.html)
+ [DeleteResourceDataSync](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteResourceDataSync.html)
+ [ListResourceDataSync](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ListResourceDataSync.html)
+ [UpdateResourceDataSync](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateResourceDataSync.html)

A delegated administrator can create a maximum of five resource data syncs for either an entire organization or a subset of organizational units\. Resource data syncs created by a delegated administrator are only available in the delegated administrator account\. You can't view the syncs or the aggregated data in the AWS Organizations management account\.

For more information about resource data sync, see [Setting up Systems Manager Explorer to display data from multiple accounts and Regions](Explorer-resource-data-sync.md)\. For more information about AWS Organizations, see [What is AWS Organizations?](https://docs.aws.amazon.com/organizations/latest/userguide/) in the *AWS Organizations User Guide*\.

**Topics**
+ [Configure an Explorer delegated administrator](#Explorer-setup-delegated-administrator-configure)
+ [Deregister an Explorer delegated administrator](#Explorer-setup-delegated-administrator-deregister)

## Configure an Explorer delegated administrator<a name="Explorer-setup-delegated-administrator-configure"></a>

Use the following procedure to register an Explorer delegated administrator\.

**To register an Explorer delegated administrator**

1. Log into your AWS Organizations management account\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Choose **Settings**\.

1. In the **Delegated administrator for Explorer** section, verify that you have configured the required service\-linked role and service access options\. If necessary, choose the **Create role** and **Enable access** buttons to configure these options\.

1. For **Account ID**, enter the AWS account ID\. This account must be a member account in AWS Organizations\.

1. Choose **Register delegated administrator**\.

The delegated administrator now has access to the **Include all accounts from my AWS Organizations configuration** and **Select organization units in AWS Organizations** options on the **Create resource data sync** page\. 

## Deregister an Explorer delegated administrator<a name="Explorer-setup-delegated-administrator-deregister"></a>

Use the following procedure to deregister an Explorer delegated administrator\. A delegated administrator account can only be deregistered by the AWS Organizations management account\. When a delegated administrator account is deregistered, the system deletes all AWS Organizations resource data syncs created by the delegated administrator\.

**To deregister an Explorer delegated administrator**

1. Log into your AWS Organizations management account\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Choose **Settings**\.

1. In the **Delegated administrator for Explorer** section, choose **Deregister**\. The system displays a warning\.

1. Enter the account ID and choose **Remove**\.

The account no longer has access to the AWS Organizations resource data sync API operations\. The system deletes all AWS Organizations resource data syncs created by the account\.