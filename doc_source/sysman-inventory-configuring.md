# Configuring Inventory Collection<a name="sysman-inventory-configuring"></a>

This section describes how to configure inventory collection on one or more managed instances by using the Systems Manager console\. For an example of how to configure inventory collection by using the AWS CLI, see [Systems Manager Inventory Walkthroughs](sysman-inventory-walk.md)\.

When you configure inventory collection, you start by creating a Systems Manager State Manager association\. Systems Manager collects the inventory data when the association is run\. If you don't create the association first, and attempt to invoke the aws:softwareInventory plugin by using, for example, Run Command, the system returns the following error:

```
The aws:softwareInventory plugin can only be invoked via ssm-associate.
```

Also note that an instance can have only have one Inventory association configured at a time\. If you configure an instance with two or more associations, the Inventory association doesn't run and no inventory data is collected\.

**Before You Begin**  
Before you configure inventory collection, complete the following tasks\.
+ Update SSM Agent on the instances you want to inventory\. By running the latest version of SSM Agent, you ensure that you can collect metadata for all supported inventory types\. For information about how to update SSM Agent by using State Manager, see [Automatically Update SSM Agent \(CLI\)](sysman-state-cli.md)\.
+ Verify that your instances meet Systems Manager prerequisites\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.
+ \(Optional\) Create a resource data sync to centrally store Inventory data in an Amazon S3 bucket\. Resource data sync then automatically updates the centralized data when new Inventory data is collected\. For more information, see [Configuring Resource Data Sync for Inventory](sysman-inventory-datasync.md)\.
+ \(Optional\) Create a JSON file to collect custom inventory\. For more information, see [Working with Custom Inventory](sysman-inventory-custom.md)\.

## Inventory All Managed Instances in Your AWS Account<a name="inventory-management-inventory-all"></a>

You can easily inventory all managed instances in your AWS account by creating a global inventory association\. A global inventory association performs the following actions:
+ Automatically applies the global inventory configuration \(association\) to all existing managed instances in your AWS account\. Instances that already have an inventory association are skipped when the global inventory association is applied and runs\. When an instance is skipped, the detailed status message states `Overridden By Explicit Inventory Association`\. Those instances are skipped by the global association, but they will still report inventory when they run their assigned inventory association\.
+ Automatically adds new instances created in your AWS account to the global inventory association\.

**Note**  
If an instance is configured for the global inventory association, and you assign a specific association to that instance, then Systems Manager Inventory deprioritizes the global association and applies the specific association\.
Global inventory associations are available in SSM Agent version 2\.0\.790\.0 or later\. For information about how to update SSM Agent on your instances, see [Update SSM Agent by using Run Command](rc-console.md#rc-console-agentexample)\.

### Configuring Inventory Collection with One Click \(Console\)<a name="sysman-inventory-config-collection-one-click"></a>

Use the following procedure to configure all managed instances in your AWS account and in a single AWS Region for Inventory with one click from the Systems Manager console\.

**To configure all of your managed instances in the current Region for Systems Manager Inventory**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Inventory**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Inventory** in the navigation pane\.

1. In the **Managed instances with inventory enabled** card, choose **Click here to enable inventory on all instances**\.  
![\[Enabling Systems Manager Inventory on all instances in the current AWS account and Region.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-one-click-1.png)

   If successful, the console displays the following message\.  
![\[Enabling Systems Manager Inventory on all instances in the current AWS account and Region.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-one-click-2.png)

   Depending on the number of managed instances in your account, it can take several minutes for the global inventory association to be applied\. Wait a few minutes and then refresh the page\. Verify that the graphic changes to reflect that inventory is configured on all of your managed instances\.

## Configuring Collection by Using the Console<a name="sysman-inventory-config-collection"></a>

This section includes information about how to configure Systems Manager Inventory to collect metadata from your managed instances by using the Systems Manager console\. You can quickly collect metadata from all instances in a specific AWS account \(and any future instances that might be created in that account\) or you can selectively collect Inventory data by using tags or instance IDs\.

**To configure inventory collection**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Inventory**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Inventory** in the navigation pane\.

1. Choose **Setup Inventory**\.

1. In the **Targets** section, identify the instances where you want to run this operation by choosing one of the following\.
   + **Selecting all managed instances in this account** \- This option selects all managed instances for which there is no existing inventory association\. If you choose this option, instances that already had inventory associations are skipped during inventory collection, and shown with a status of **Skipped** in inventory results\. For more information, see [Inventory All Managed Instances in Your AWS Account](#inventory-management-inventory-all)\. 
   + **Specifying a tag** \- This option lets you specify a single tag to identify instances in your account from which you want to collect inventory\. If you use a tag, any instances created in the future with the same tag will also report inventory\. If there is an existing inventory association with all instances, using a tag to select specific instances as a target for a different inventory overrides instance membership in the **All managed instances** target group\. Instances with the specified tag are skipped on future inventory collection from **All managed instances**\.
   + **Manually selecting instances** \- This option lets you choose specific managed instances in your account\. Explicitly choosing specific instances by using this option overrides inventory associations on the **All managed instances** target\. The instance is skipped on future inventory collection from **All managed instances**\.

1. In the **Schedule** section, choose how often you want the system to collect inventory metadata from your instances\.

1. In the **Parameters** section, use the lists to enable or disable different types of inventory collection\. See the following samples if you want to create an inventory search for **Files** or the **Windows Registry**\.

**Files**
   + On Linux, collect metadata of \.sh files in the `/home/ec2-user` directory, excluding all subdirectories\.

     ```
     [{"Path":"/home/ec2-user","Pattern":["*.sh", "*.sh"],"Recursive":false}]
     ```
   + On Windows, collect metadata of all "\.exe" files in the Program Files folder, including subdirectories recursively\.

     ```
     [{"Path":"C:\Program Files","Pattern":["*.exe"],"Recursive":true}]
     ```
   + On Windows, collect metadata of specific log patterns\.

     ```
     [{"Path":"C:\ProgramData\Amazon","Pattern":["*amazon*.log"],"Recursive":true}]
     ```
   + Limit the directory count when performing recursive collection\.

     ```
     [{"Path":"C:\Users","Pattern":["*.ps1"],"Recursive":true, "DirScanLimit": 1000}]
     ```

**Windows Registry**
   + Collect all keys and values recursively for a specific path\.

     ```
     [{"Path":"HKEY_LOCAL_MACHINE\SOFTWARE\Amazon","Recursive": true}]
     ```
   + Collect all keys and values for a specific path \(recursive search disabled\)\.

     ```
     [{"Path":"HKEY_LOCAL_MACHINE\SOFTWARE\Intel\PSIS\PSIS_DECODER", "Recursive": false}]
     ```
   + Collect a specific key by using the `ValueNames` option\.

     ```
     {"Path":"HKEY_LOCAL_MACHINE\SOFTWARE\Amazon\MachineImage","ValueNames":["AMIName"]}
     ```

   For more information about collecting File and Windows Registry inventory, see [Working with File and Windows Registry Inventory](sysman-inventory-file-and-registry.md)\.

1. In the **Advanced** section, choose **Sync inventory execution logs to an S3 bucket** if you want to store the association execution status in an Amazon S3 bucket\.

1. Choose **Setup Inventory**\. Systems Manager creates a State Manager association and immediately runs Inventory on the instances\.

1. In the navigation pane, choose **State Manager**\. Verify that a new association was created that uses the **AWS\-GatherSoftwareInventory** document\. The association schedule uses a rate expression\. Also, verify that the **Status** field shows **Success**\. If you chose the option to **Sync inventory execution logs to an S3 bucket**, then you can view the log data in Amazon S3 after a few minutes\. If you want to view inventory data for a specific instance, then choose **Managed Instances** in the navigation pane\. 

1. Choose an instance, and then choose **View details**\.

1. On the instance details page, choose **Inventory**\. Use the **Inventory type** lists to filter the inventory\.