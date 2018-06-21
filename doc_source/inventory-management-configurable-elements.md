# Configurable Data Types in Inventory Management<a name="inventory-management-configurable-elements"></a>

The following table describes the different aspects of Inventory collection that you configure\.


****  

| Configuration | Details | 
| --- | --- | 
|  Type of information to collect  | You can configure Inventory to collect the following types of metadata: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/inventory-management-configurable-elements.html)  To view a list of all metadata collected by Inventory, see [Metadata Collected by Inventory](sysman-inventory-schema.md)\.   | 
|  Instances to collect information from  |  You can choose to inventory all instances in your AWS account, individually select instances, or target groups of instances by using Amazon EC2 tags\. For more information about performing inventory collection on all of your instances, see [Inventory All Managed Instances in Your AWS Account](inventory-management-inventory-all.md)\.  | 
|  When to collect information  |  You can specify a collection interval in terms of minutes, hours, days, and weeks\. The shortest collection interval is every 30 minutes\.   | 

Depending on the amount of data collected, the system can take several minutes to report the data to the output you specified\. After the information is collected, the metadata is sent over a secure HTTPS channel to a plain\-text AWS store that is accessible only from your AWS account\. You can view the data in the Amazon S3 bucket you specified, or in the Amazon EC2 console on the **Inventory** tab for your managed instance\. The **Inventory** tab includes several predefined filters to help you query the data\.

To start collecting inventory on your managed instance, see [Configuring Inventory Collection](sysman-inventory-configuring.md) and [Walkthrough: Use the AWS CLI to Collect Inventory](sysman-inventory-cliwalk.md)\.