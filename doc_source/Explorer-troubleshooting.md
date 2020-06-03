# Troubleshooting Systems Manager Explorer<a name="Explorer-troubleshooting"></a>

This topic includes information about how to troubleshoot common problems with Systems Manager Explorer\.

 **The AWS Organizations options on the *Create resource data sync* page are greyed out** 

The following options on the **Create resource data sync** page are only available if you set up and configured AWS Organizations\. If you set up and configured AWS Organizations, then either the AWS Organizations master account or an Explorer delegated administrator can create resource data syncs that use these options\. 

![\[AWS Organizations options for Explorer resource data sync.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/explorer-delegated-administration-2.png)

For more information, see [Setting up Systems Manager Explorer to display data from multiple accounts and Regions](Explorer-resource-data-sync.md) and [Configuring a Delegated Administrator](Explorer-setup-delegated-administrator.md)\.

 **Explorer doesn't display any data at all** 
+ Verify that you completed Integrated Setup in each account and Region where you want Explorer to access and display data\. If you don't, Explorer won't display OpsData and OpsItems for those accounts and Regions in which you didn't complete Integrated Setup\. For more information, see [Getting started with Systems Manager Explorer and OpsCenter](Explorer-setup.md)\.
+ When using Explorer to view data from multiple accounts and Regions, verify that you are logged into the AWS Organizations Master Account\. To view OpsData and OpsItems from multiple accounts and Regions, you must be signed in to this account\.

 **Widgets about EC2 instances don't display data** 

If widgets about EC2 instances, such as the **Instance count**, **Managed instances**, and **Instance by AMI** widgets don't display data, then verify the following:
+ Verify that you waited several minutes\. OpsData can take several minutes to display in Explorer after you completed Integrated Setup\.
+ Verify that you configured AWS Config configuration recorder\. Explorer uses data provided by AWS Config configuration recorder to populate widgets with information about your EC2 instances\. For more information, see [Managing the Configuration Recorder](https://docs.aws.amazon.com/config/latest/developerguide/stop-start-recorder.html)\.
+ Verify that the **Amazon EC2** OpsData source is enabled on the **Settings** page\. Also, verify that more than 6 hours have passed since you enabled configuration recorder or since you made changes to your instances\. Systems Manager can take up to six hours to display data from AWS Config in Explorer EC2 widgets after you initially enable configuration recorder or make changes to your instances\.
+ Be aware that if an instance is either stopped or terminated, then Explorer stops showing those instances after 24 hours\.
+ Verify that you are in the correct AWS Region where you configured your EC2 instances\. Explorer doesn't display data about on\-premises instances\.
+ If you configured a resource data sync for multiple accounts and Regions, verify that you are signed in to the Organizations master account\. 

 **Patch widget doesn't display data** 

The **Non\-compliant instances for patching** widget only displays data about patch instances that are not compliant\. This widget displays no data if your instances are compliant\. If you suspect that you have non\-compliant instances, then verify that you set up and configured Systems Manager patching and use Patch Manager to check your patch compliance\. For more information, see [AWS Systems Manager Patch Manager](systems-manager-patch.md)\.

 **Miscellaneous issues** 

**Explorer doesn't let you edit or remediate OpsItems**: OpsItems viewed across accounts or Regions are read\-only\. They can only be updated and remediated from their home account or Region\.