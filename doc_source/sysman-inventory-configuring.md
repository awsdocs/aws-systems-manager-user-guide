# Configuring Inventory Collection<a name="sysman-inventory-configuring"></a>

This section describes how to configure inventory collection on one or more managed instances by using the Amazon EC2 console\. This section also describes how to aggregate inventory data from multiple AWS accounts and regions in a single Amazon S3 bucket by using Systems Manager Resource Data Sync\. For an example of how to configure inventory collection using the AWS CLI, see [Systems Manager Inventory Manager Walkthroughs](sysman-inventory-walk.md)\.

**Before You Begin**  
Before you configure inventory collection, complete the following tasks\.

+ Update SSM Agent on the instances you want to inventory\. By running the latest version of SSM Agent, you ensure that you can collect metadata for all supported inventory types\. For information about how to update SSM Agent by using State Manager, see [Walkthrough: Automatically Update the SSM Agent \(CLI\)](sysman-state-cli.md)\.

+ Verify that your instances meet Systems Manager prerequisites\. For more information, see [Systems Manager Prerequisites](systems-manager-setting-up.md#systems-manager-prereqs)\.

+ \(Optional\) Create a JSON file to collect custom inventory\. For more information, see [Working with Custom Inventory](sysman-inventory-about.md#sysman-inventory-custom)\.

## Configuring Collection<a name="sysman-inventory-config-collection"></a>

Use the following procedure to configure inventory collection on a managed instance using the console\.

**Note**  
When you configure inventory collection, you start by creating a Systems Manager State Manager association\. Systems Manager collects the inventory data when the association is run\. If you don't create the association first, and attempt to invoke the aws:softwareInventory plugin by using, for example, Run Command, the system returns the following error:  

```
The aws:softwareInventory plugin can only be invoked via ssm-associate.
```
Also note that an instance can have only have one Inventory association configured at a time\. If you configure an instance with two or more associations, the association doesn't run and no inventory data is collected\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To configure inventory collection \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Inventory**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Inventory** in the navigation pane\.

1. Choose **Setup Inventory**\.

1. In the **Targets** section, identify the instances where you want to run this operation by specifying tags or selecting instances manually\.
**Note**  
If you use tags, any instances created in the future with the same tag will also report inventory\.

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

   For more information about collecting File and Windows Registry inventory, see [Working with File and Windows Registry Inventory](sysman-inventory-about.md#sysman-inventory-file-and-registry)\.

1. In the **Advanced** section, choose **Sync inventory execution logs to an S3 bucket** if you want to store the association execution status in an Amazon S3 bucket\.

1. Choose **Setup Inventory**\. Systems Manager creates a State Manager association an immediately runs Inventory on the specified or targeted instances\.

1. In the navigation pane, choose **State Manager**\. Verify that a new association was created that uses the **AWS\-GatherSoftwareInventory** document\. Also, verify that the **Status** field shows **Success**\. If you chose the option to **Sync inventory execution logs to an S3 bucket**, then you can view the log data in Amazon S3 after a few minutes\. If you want to view inventory data for a specific instance, then choose **Managed Instances** in the navigation pane\. 

1. Choose an instance, and then choose **View details**\.

1. On the instance details page, choose **Inventory**\. Use the **Inventory type** lists to filter the inventory\.

**To configure inventory collection \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Shared Resources** in the navigation pane, and then choose **Managed Instances**\. 

1. Choose **Setup Inventory**\.

1. In the **Targets** section, choose **Specify a Tag** if you want to configure inventory on multiple instances using EC2 tags\. Choose **Manually Select Instances** if you want to individually choose which instances are configured for inventory\.
**Note**  
If you use tags, any instances created in the future with the same tag will also report inventory\.

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

   For more information about collecting File and Windows Registry inventory, see [Working with File and Windows Registry Inventory](sysman-inventory-about.md#sysman-inventory-file-and-registry)\.

1. In the **Advanced** section, choose **Write to S3** if you want to store the association execution status in an Amazon S3 bucket\.

1. Choose **Setup Inventory** and then choose **OK**\.

1. In the **Managed Instances** page, choose an instance that you just configured for inventory and choose the **Description** tab\. The **Association Status** shows **Pending** until the association is created\. If the status is **Failed**, verify that you have the latest version of the SSM Agent installed on your instances\.

1. After the collection time\-frame has passed, choose a managed instance, and then choose the **Inventory** tab\.

1. Use the **Inventory Type** list to filter on different types of inventory data\.

After you configure Inventory collection, we recommend that you configure Systems Manager Resource Data Sync\. Resource Data Sync centralizes all Inventory data in a target Amazon S3 bucket and automatically updates the central storage when new Inventory data is collected\. With all Inventory data stored in a target Amazon S3 bucket, you can then use services like Amazon Athena and Amazon QuickSight to query and analyze the aggregated data\. For more information, see [Configuring Resource Data Sync for Inventory](sysman-inventory-datasync.md)\.