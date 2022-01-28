# AWS Systems Manager Compliance<a name="systems-manager-compliance"></a>

You can use Compliance, a capability of AWS Systems Manager, to scan your fleet of managed nodes for patch compliance and configuration inconsistencies\. You can collect and aggregate data from multiple AWS accounts and Regions, and then drill down into specific resources that aren’t compliant\. By default, Compliance displays current compliance data about patching in Patch Manager and associations in State Manager\. \(Patch Manager and State Manager are also both capabilities of AWS Systems Manager\.\)

Patch compliance data from Patch Manager can be sent to AWS Security Hub\. Security Hub gives you a comprehensive view of your high\-priority security alerts and compliance status\. It also monitors the patching status of your fleet\. For more information, see [Integrating Patch Manager with AWS Security Hub](patch-manager-security-hub-integration.md)\. 

Compliance offers the following additional benefits and features: 
+ View compliance history and change tracking for Patch Manager patching data and State Manager associations by using AWS Config\.
+ Customize Compliance to create your own compliance types based on your IT or business requirements\.
+ Remediate issues by using Run Command, another capability of AWS Systems Manager, State Manager, or Amazon EventBridge\.
+ Port data to Amazon Athena and Amazon QuickSight to generate fleet\-wide reports\.



**EventBridge support**  
This Systems Manager capability is supported as an *event* type in Amazon EventBridge rules\. For information, see [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md) and [Reference: Amazon EventBridge event patterns and types for Systems Manager](reference-eventbridge-events.md)\.

**Chef InSpec integration**  
Systems Manager integrates with [Chef InSpec](https://www.chef.io/inspec/)\. InSpec is an open\-source, runtime framework that allows you to create human\-readable profiles on GitHub or Amazon Simple Storage Service \(Amazon S3\)\. You can then use Systems Manager to run compliance scans and view compliant and noncompliant managed nodes\. For more information, see [Using Chef InSpec profiles with Systems Manager Compliance](integration-chef-inspec.md)\.

**Pricing**  
Compliance is offered at no additional charge\. You only pay for the AWS resources that you use\.

**Topics**
+ [Getting started with Compliance](sysman-compliance-prereqs.md)
+ [Creating a resource data sync for Compliance](sysman-compliance-datasync-create.md)
+ [Working with Compliance](sysman-compliance-about.md)
+ [Deleting a resource data sync for Compliance](#systems-manager-compliance-delete-RDS)
+ [Remediating compliance issues using EventBridge](sysman-compliance-fixing.md)
+ [Compliance walkthrough \(AWS CLI\)](sysman-compliance-walk.md)

## Deleting a resource data sync for Compliance<a name="systems-manager-compliance-delete-RDS"></a>

If you no longer want to use AWS Systems Manager Compliance to view compliance data, then we also recommend deleting resource data syncs used for Compliance data collection\.

**To delete a Compliance resource data sync**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose **Account management**, **Resource data syncs**\.

1. Choose a sync in the list\. 
**Important**  
Make sure you choose the sync used for Compliance\. Systems Manager supports resource data sync for multiple capabilities\. If you choose the wrong sync, you could disrupt data aggregation for Systems Manager Explorer or Systems Manager Inventory\.

1. Choose **Delete**\.

1. Delete the Amazon Simple Storage Service \(Amazon S3\) bucket where the data was stored\. For information about deleting an Amazon S3 bucket, see [Deleting a bucket](https://docs.aws.amazon.com/AmazonS3/latest/dev/delete-bucket.html)\.