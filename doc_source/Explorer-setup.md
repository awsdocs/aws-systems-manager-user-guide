# Getting started with Systems Manager Explorer and OpsCenter<a name="Explorer-setup"></a>

Systems Manager uses an integrated setup experience to help you get started with Systems Manager Explorer and Systems Manager OpsCenter\. In this documentation, Explorer and OpsCenter Setup is called *Integrated Setup*\. If you already set up OpsCenter, you still need to complete Integrated Setup to verify settings and options\. If you have not set up OpsCenter, then you can use Integrated Setup to get started with both capabilities\. Integrated Setup performs the following tasks\.

1. [Configures roles and permissions](Explorer-setup-permissions.md): Integrated Setup creates an AWS Identity and Access Management \(IAM\) role that enables Amazon CloudWatch Events to automatically create OpsItems based on default rules\. After setting up, you must configure IAM user, group, or role permissions for OpsCenter, as described in this section\. 

1. [Enables default rules for OpsItem creation](Explorer-setup-default-rules.md): Integrated Setup creates default rules in CloudWatch Events\. These rules automatically create OpsItems in response to events\. Examples of these events are: state change for an AWS resource, a change in security settings, or a service becoming unavailable\.

1. [Enables OpsData sources](Explorer-setup-data-sources.md): Integrated Setup enables data sources that populate Explorer widgets\.

1. [Enables you to specify reporting tag keys](Explorer-setup-tag-keys.md): Integrated Setup enables you to specify up to five reporting tag keys to automatically assign to new OpsItems that meet specific criteria\. 

After you complete Integrated Setup, we recommend that you [Set up Explorer to display data from multiple Regions and accounts](Explorer-resource-data-sync.md)\. Explorer and OpsCenter automatically synchronize OpsData and OpsItems for the AWS account and Region you used when you completed Integrated Setup\. You can aggregate OpsData and OpsItems from other accounts and Regions by creating a resource data sync\.

**Note**  
You can change setup configurations at any time on the **Settings** page\.

## Before you begin<a name="Explorer-setup-related-services"></a>

Explorer and OpsCenter collect information from, or interact with, other AWS services and Systems Manager capabilities\. We recommend that you set up and configure these other services or capabilities before you use Integrated Setup\.

The following table includes tasks that enable Explorer and OpsCenter to collect information from, or interact with, other AWS services and Systems Manager capabilities\. 


****  

| Task | Information | 
| --- | --- | 
|  Verify permissions in Systems Manager Automation  |  Explorer and OpsCenter enable you to remediate issues with AWS resources by using Systems Manager Automation documents \(runbooks\)\. To use this remediation capability, you must have permission to run Systems Manager Automation documents\. For more information, see [Getting started with Automation](automation-setup.md)\.  | 
|  Set up and configure Systems Manager Patch Manager  |  Explorer includes a widget that provides information about patch compliance\. To view data in this widget, you must configure patching\. For more information, see [AWS Systems Manager Patch Manager](systems-manager-patch.md)\.  | 
|  Enable AWS Config Configuration Recorder  |  Explorer uses data provided by AWS Config configuration recorder to populate widgets with information about your EC2 instances\. To ensure that these widgets display data, enable AWS Config configuration recorder\. For more information, see [Managing the Configuration Recorder](https://docs.aws.amazon.com/config/latest/developerguide/stop-start-recorder.html)\.  After you enable configuration recorder, Systems Manager can take up to six hours to display data in Explorer widgets that display information about your EC2 instances\.   | 