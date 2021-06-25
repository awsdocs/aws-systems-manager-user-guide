# Getting started with Systems Manager Explorer and OpsCenter<a name="Explorer-setup"></a>

AWS Systems Manager uses an integrated setup experience to help you get started with Systems Manager Explorer and Systems Manager OpsCenter\. In this documentation, Explorer and OpsCenter Setup is called *Integrated Setup*\. If you already set up OpsCenter, you still need to complete Integrated Setup to verify settings and options\. If you haven't set up OpsCenter, then you can use Integrated Setup to get started with both capabilities\.

**Note**  
Integrated Setup is only available in the Systems Manager console\. You can't set up Explorer or OpsCenter programmatically\.

Integrated Setup performs the following tasks:
+ [Configures roles and permissions](Explorer-setup-permissions.md): Integrated Setup creates an AWS Identity and Access Management \(IAM\) role that enables Amazon EventBridge to automatically create OpsItems based on default rules\. After setting up, you must configure IAM user, group, or role permissions for OpsCenter, as described in this section\. 
+ [Enables default rules for OpsItem creation](Explorer-setup-default-rules.md): Integrated Setup creates default rules in EventBridge\. These rules automatically create OpsItems in response to events\. Examples of these events are: state change for an AWS resource, a change in security settings, or a service becoming unavailable\.
+ [Enables OpsData sources](#Explorer-setup-data-sources): Integrated Setup enables data sources that populate Explorer widgets\.
+ [Enables you to specify reporting tag keys](Explorer-setup-tag-keys.md): Integrated Setup enables you to specify up to five reporting tag keys to automatically assign to new OpsItems that meet specific criteria\. 

After you complete Integrated Setup, we recommend that you [Set up Explorer to display data from multiple Regions and accounts](Explorer-resource-data-sync.md)\. Explorer and OpsCenter automatically synchronize OpsData and OpsItems for the AWS account and AWS Region you used when you completed Integrated Setup\. You can aggregate OpsData and OpsItems from other accounts and Regions by creating a resource data sync\.

**Note**  
You can change setup configurations at any time on the **Settings** page\.

## Configuring OpsData sources<a name="Explorer-setup-data-sources"></a>

Integrated Setup enables the following data sources that populate Explorer widgets\.
+ AWS Support Center \(You must have either a Business or Enterprise Support plan to enable this source\.\)
+ AWS Compute Optimizer \(You must have either a Business or Enterprise Support plan to enable this source\.\)
+ Systems Manager State Manager association compliance
+ AWS Config Compliance
+ Systems Manager OpsCenter
+ Systems Manager Patch Manager patch compliance
+ Amazon Elastic Compute Cloud \(Amazon EC2\)
+ Systems Manager Inventory
+ AWS Trusted Advisor \(You must have either a Business or Enterprise Support plan to enable this source\.\)
+ AWS Security Hub