# Systems Manager Inventory Manager Walkthroughs<a name="sysman-inventory-walk"></a>

Use the following walkthroughs to collect and manage Inventory data\. We recommend that you initially perform these walkthroughs with managed instances in a test environment\. 

**Before You Begin**  
Before you start these walkthroughs, complete the following tasks\.
+ Update SSM Agent on the instances you want to inventory\. By running the latest version of SSM Agent, you ensure that you can collect metadata for all supported inventory types\. For information about how to update SSM Agent by using State Manager, see [Walkthrough: Automatically Update SSM Agent \(CLI\)](sysman-state-cli.md)\.
+ Verify that your instances meet Systems Manager prerequisites\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.
+ \(Optional\) Create a JSON file to collect custom inventory\. For more information, see [Working with Custom Inventory](sysman-inventory-custom.md)\.

**Topics**
+ [Walkthrough: Assign Custom Inventory Metadata to an Instance](sysman-inventory-walk-custom.md)
+ [Walkthrough: Use the AWS CLI to Collect Inventory](sysman-inventory-cliwalk.md)
+ [Walkthrough: Use Resource Data Sync to Aggregate Inventory Data](sysman-inventory-resource-data-sync.md)