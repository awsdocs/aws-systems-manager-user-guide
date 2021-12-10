# AWS Systems Manager Inventory<a name="systems-manager-inventory"></a>

AWS Systems Manager Inventory provides visibility into your Amazon EC2 and on\-premises computing environment\. You can use Inventory to collect *metadata* from your managed nodes\. You can store this metadata in a central Amazon Simple Storage Service \(Amazon S3\) bucket, and then use built\-in tools to query the data and quickly determine which nodes are running the software and configurations required by your software policy, and which nodes need to be updated\. You can configure Inventory on all of your managed nodes by using a one\-click procedure\. You can also configure and view inventory data from multiple AWS Regions and AWS accounts\.

If the pre\-configured metadata types collected by Systems Manager Inventory don't meet your needs, then you can create custom inventory\. Custom inventory is simply a JSON file with information that you provide and add to the managed node in a specific directory\. When Systems Manager Inventory collects data, it captures this custom inventory data\. For example, if you run a large data center, you can specify the rack location of each of your servers as custom inventory\. You can then view the rack space data when you view other inventory data\.

**Important**  
Systems Manager Inventory collects *only* metadata from your managed nodes\. Inventory doesn't access proprietary information or data\.

The following table lists the types of metadata that you can collect with Systems Manager Inventory\. The table also lists the nodes you can collect inventory information from and the collection intervals you can specify\.


****  

| Configuration | Details | 
| --- | --- | 
|  Metadata types  |  You can configure Inventory to collect the following types of metadata: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-inventory.html)  To view a list of all metadata collected by Inventory, see [Metadata collected by inventory](sysman-inventory-schema.md)\.   | 
|  Nodes to collect information from  |  You can choose to inventory all managed nodes in your AWS account, individually select nodes, or target groups of nodes by using tags\. For more information about performing inventory collection on all of your nodes, see [Inventory all managed nodes in your AWS account](sysman-inventory-configuring.md#inventory-management-inventory-all)\.  | 
|  When to collect information  |  You can specify a collection interval in terms of minutes, hours, and days\. The shortest collection interval is every 30 minutes\.   | 

**Note**  
Depending on the amount of data collected, the system can take several minutes to report the data to the output you specified\. After the information is collected, the metadata is sent over a secure HTTPS channel to a plain\-text AWS store that is accessible only from your AWS account\. 

You can view the data in the Systems Manager console on the **Inventory** page, which includes several predefined cards to help you query the data\.

![\[Systems Manager Inventory cards in the Systems Manager console.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-cards.png)

**Note**  
Inventory cards automatically filter out Amazon EC2 managed instances with a state of *Terminated* and *Stopped*\. For on\-premises and AWS IoT Greengrass core device managed nodes, Inventory cards automatically filter out nodes with a state of *Terminated*\. 

If you create a resource data sync to synchronize and store all of your data in a single Amazon S3 bucket, then you can drill down into the data on the **Inventory Detailed View** page\. For more information, see [Querying inventory data from multiple Regions and accounts](systems-manager-inventory-query.md)\.

**EventBridge support**  
This Systems Manager capability is supported as an *event* type in Amazon EventBridge rules\. For information, see [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md) and [Reference: Amazon EventBridge event patterns and types for Systems Manager](reference-eventbridge-events.md)\.

**Topics**
+ [Learn more about Systems Manager Inventory](sysman-inventory-about.md)
+ [Setting up Systems Manager Inventory](systems-manager-inventory-setting-up.md)
+ [Configuring inventory collection](sysman-inventory-configuring.md)
+ [Working with Systems Manager inventory data](systems-manager-inventory-data-working.md)
+ [Working with custom inventory](sysman-inventory-custom.md)
+ [Viewing inventory history and change tracking](sysman-inventory-history.md)
+ [Stopping data collection and deleting inventory data](systems-manager-inventory-delete.md)
+ [Systems Manager Inventory walkthroughs](sysman-inventory-walk.md)
+ [Troubleshooting problems with Systems Manager Inventory](syman-inventory-troubleshooting.md)