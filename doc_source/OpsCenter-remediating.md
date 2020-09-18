# Remediating OpsItem issues using Systems Manager Automation<a name="OpsCenter-remediating"></a>

AWS Systems Manager Automation helps you quickly remediate issues with AWS resources identified in your OpsItems\. Automation uses predefined SSM Automation documents \(runbooks\) to remediate commons issues with AWS resources\. For example, Automation includes runbooks to perform the following actions: 
+ Stop, start, restart, and terminate Amazon Relational Database Service \(Amazon RDS\) and Amazon Elastic Compute Cloud \(EC2\) instances\.
+ Create AWS resources such as Amazon Machine Images \(AMIs\), Amazon Elastic Block Store \(Amazon EBS\) snapshots, and Amazon DynamoDB backups\.
+ Configure a resource to use AWS services, including Amazon EventBridge, AWS CloudTrail, and Amazon Simple Storage Service \(Amazon S3\) bucket logging and versioning\.
+ Attach an AWS Identity and Access Management \(IAM\) instance profile to an instance\.
+ Troubleshoot RDP and SSH connectivity issues for EC2 instances\.
+ Reset access for an EC2 instance\.

Each OpsItem in the AWS Management Console includes a **Runbooks** section, as shown in the following example\.

![\[A list of Systems Manager Automation runbooks for remediating OpsItem issues\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_automation_runbook_list.png)

## Automation document features in OpsCenter<a name="OpsCenter-remediating-features"></a>

The following list describes some of the features available to help you run Automation documents and remediate issues\.
+ When you choose an AWS resource that generated an OpsItem, OpsCenter displays a list of Automation documents that you can run on that resource, as shown in the following example\.   
![\[A refined list of Systems Manager Automation documents that can run on an AWS resource that generated an OpsItem.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_automation_refined_runbooks.png)
+ When you choose an Automation document from the list, OpsCenter prepopulates some of the fields required to run the document\. In the following example, OpsCenter prepopulated the **VolumeId** field\.   
![\[A prepopulated field to help you quickly run an Automation document.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_automation_prepopulated_field.png)
+ OpsCenter keeps a 30\-day record of Automation documents run for a specific OpsItem, as shown in the following example\.  
![\[A 30-day history of Systems Manager Automation documents run for an OpsItem.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_automation_history.png)
+ In the **Status and results** column, you can choose a status to view important details about that run, such as the reason why an Automation failed and which step of the Automation document was running when the failure occurred, as shown in the following example\.  
![\[Status information for the last time an Automation document was run.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_automation_results.png)
+ The **Related resource details** page for a selected OpsItem includes the **Run automation** list\. The list enables you to choose recent or resource\-specific Automation documents that you can run to remediate issues\. This page also includes helpful data providers, including Amazon CloudWatch metrics and alarms, AWS CloudTrail logs, and details from AWS Config, to name a few\.  
![\[List of available Automation documents you can run and metrics available on the Related Resources tab.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_automation_related_resource_details.png)
+ You can view information about an Automation document by either choosing its name in the console or by using the [ Automation document details reference](automation-documents-reference-details.md)\.

## Using a runbook to remediate an OpsItem issue<a name="OpsCenter-remediating-how-to"></a>

When you run a Systems Manager Automation document \(runbook\) from an OpsItem, you can run a simple version or you can choose the **Advanced configuration** option\. The **Advanced configuration** opens the runbook in Systems Manager Automation, which provides several options for executing the runbook\.

![\[An OpsCenter runbook that uses the Advanced Configuration and opens in Systems Manager Automation\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_automation_runbook_advanced.png)

The following procedure describes how to run a simple version of a runbook\. For information about executing an **Advanced configuration** runbook, see [Working with automations](automation-working.md)\.

**Before You Begin**  
Before you run an automation document \(runbook\) to remediate an OpsItem issue, do the following:
+ Verify that you have permission to run Systems Manager Automation documents\. For more information, see [Getting started with Automation](automation-setup.md)\.
+ Collect resource\-specific ID information for the automation that you want to run\. For example, if you want to run an automation that restarts an EC2 instance, then you must specify the ID of the instance to restart\.

**To run an Automation document \(runbook\) to remediate an OpsItem issue**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose the OpsItem ID to open the details page\.  
![\[A new OpsItem on the OpsCenter Overview page\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_1.png)

1. Scroll to the **Runbooks** section\.

1. Use the Runbooks Search bar or the numbers in the upper right to find the Automation document that you want to run\.  
![\[Using controls to locate an OpsCenter runbook.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_automation_runbook_list_2.png)

1. Choose a runbook, and then choose **Execute**\.

1. Enter the required information for the runbook, and then choose **Execute**\.

1. In the navigation pane, choose **Automation**, and then choose the **Execution ID** link to view the steps and the status of the execution\.   
![\[Using controls to locate an OpsCenter runbook.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_automation_runbook_status.png)

## Working with associated runbooks<a name="OpsCenter-remediating-associated-runbooks"></a>

After you run an Automation document \(runbook\) from an OpsItem, the runbook is automatically associated with the related resource of that OpsItem for future reference\. Associated runbooks are ranked higher than others in the **Runbooks** list, as shown in the following\.

![\[An associated runbook in an OpsItem\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_automation_associated_runbook.png)

Associated runbooks are also available in the **Run automation** list in the **Related resources** section, as shown in the following\.

![\[Using controls to locate an OpsCenter runbook.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_automation_associated_runbook_2.png)

Use the following procedure to run an Automation document \(runbook\) that has already been associated with a related resource in an OpsItem\. For information about adding related resources, see [Working with OpsItems](OpsCenter-working-with-OpsItems.md)\.

**To run a resource\-associated runbook to remediate an OpsItem issue**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Open the OpsItem\.

1. In the **Related resources** section, choose the resource on which you want to run the Automation document \(runbook\)\.

1. Choose **Run automation**, and then choose the associated Automation document that you want to run\.

1. Enter the required information for the runbook, and then choose **Execute**\.

1. In the navigation pane, choose **Automation**, and then choose the **Execution ID** link to view the steps and the status of the execution\. 