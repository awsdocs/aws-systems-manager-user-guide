# Systems Manager Inventory walkthroughs<a name="sysman-inventory-walk"></a>

Use the following walkthroughs to collect and manage inventory data by using AWS Systems Manager Inventory\. We recommend that you initially perform these walkthroughs with managed nodes in a test environment\. 

**Before you begin**  
Before you start these walkthroughs, complete the following tasks:
+ Update AWS Systems Manager SSM Agent on the nodes you want to inventory\. By running the latest version of SSM Agent, you ensure that you can collect metadata for all supported inventory types\. For information about how to update SSM Agent by using State Manager, see [Walkthrough: Automatically update SSM Agent \(CLI\)](sysman-state-cli.md)\.
+ Verify that your nodes meet Systems Manager prerequisites\. For more information, see [Systems Manager prerequisites](systems-manager-prereqs.md)\.
+ \(Optional\) Create a JSON file to collect custom inventory\. For more information, see [Working with custom inventory](sysman-inventory-custom.md)\.

**Topics**
+ [Walkthrough: Assign custom inventory metadata to a managed node](sysman-inventory-walk-custom.md)
+ [Walkthrough: Configure your managed nodes for Inventory by using the CLI](sysman-inventory-cliwalk.md)
+ [Walkthrough: Use resource data sync to aggregate inventory data](sysman-inventory-resource-data-sync.md)