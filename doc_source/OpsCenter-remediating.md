# Remediating OpsItem issues using Systems Manager Automation<a name="OpsCenter-remediating"></a>

AWS Systems Manager Automation helps you quickly remediate issues with AWS resources identified in your OpsItems\. Automation uses predefined SSM Automation runbooks to remediate commons issues with AWS resources\. For example, Automation includes runbooks to perform the following actions: 
+ Stop, start, restart, and terminate Amazon Relational Database Service \(Amazon RDS\) and Amazon Elastic Compute Cloud \(Amazon EC2\) instances\.
+ Create AWS resources such as Amazon Machine Images \(AMIs\), Amazon Elastic Block Store \(Amazon EBS\) snapshots, and Amazon DynamoDB backups\.
+ Configure a resource to use AWS services, including Amazon EventBridge, AWS CloudTrail, and Amazon Simple Storage Service \(Amazon S3\) bucket logging and versioning\.
+ Attach an AWS Identity and Access Management \(IAM\) instance profile to an instance\.
+ Troubleshoot RDP and SSH connectivity issues for Amazon EC2 instances\.
+ Reset access for an Amazon EC2 instance\.

Each OpsItem in the AWS Management Console includes a **Runbooks** section\.

## Automation runbook features in OpsCenter<a name="OpsCenter-remediating-features"></a>

The following list describes some of the features available to help you run Automation runbooks and remediate issues\.
+ When you choose an AWS resource that generated an OpsItem, OpsCenter displays a list of Automation runbooks that you can run on that resource\. 
+ When you choose an Automation runbook from the list, OpsCenter prepopulates some of the fields required to run the document\.
+ OpsCenter keeps a 30\-day record of Automation runbooks run for a specific OpsItem\.
+ In the **Status and results** column, you can choose a status to view important details about that run, such as the reason why an Automation failed and which step of the Automation runbook was running when the failure occurred, as shown in the following example\.  
![\[Status information for the last time an Automation runbook was run.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_automation_results.png)
+ The **Related resource details** page for a selected OpsItem includes the **Run automation** list\. The list enables you to choose recent or resource\-specific Automation runbooks that you can run to remediate issues\. This page also includes helpful data providers, including Amazon CloudWatch metrics and alarms, AWS CloudTrail logs, and details from AWS Config, to name a few\.  
![\[List of available Automation runbooks you can run and metrics available on the Related Resources tab.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_automation_related_resource_details.png)
+ You can view information about an Automation runbook by either choosing its name in the console or by using the [Systems Manager Automation runbook reference](automation-documents-reference.md)\.

## Using a runbook to remediate an OpsItem issue<a name="OpsCenter-remediating-how-to"></a>

When you run a Systems Manager Automation runbook from an OpsItem, you can run a simple version or you can choose the **Advanced configuration** option\. The **Advanced configuration** opens the runbook in Systems Manager Automation, which provides several options for running the runbook\.

![\[An OpsCenter runbook that uses the Advanced Configuration and opens in Systems Manager Automation\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_automation_runbook_advanced.png)

The following procedure describes how to run a simple version of a runbook\. For information about running an **Advanced configuration** runbook, see [Working with automations](automation-working.md)\.

**Before You Begin**  
Before you run an automation document \(runbook\) to remediate an OpsItem issue, do the following:
+ Verify that you have permission to run Systems Manager Automation runbooks\. For more information, see [Setting up Automation](automation-setup.md)\.
+ Collect resource\-specific ID information for the automation that you want to run\. For example, if you want to run an automation that restarts an EC2 instance, then you must specify the ID of the instance to restart\.

**To run an Automation runbook to remediate an OpsItem issue**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose the OpsItem ID to open the details page\.  
![\[A new OpsItem on the OpsCenter Overview page\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_1.png)

1. Scroll to the **Runbooks** section\.

1. Use the Runbooks Search bar or the numbers in the upper right to find the Automation runbook that you want to run\.

1. Choose a runbook, and then choose **Execute**\.

1. Enter the required information for the runbook, and then choose **Execute**\.

1. In the navigation pane, choose **Automation**, and then choose the **Execution ID** link to view the steps and the status of the execution\. 

## Working with associated runbooks<a name="OpsCenter-remediating-associated-runbooks"></a>

After you run an Automation runbook from an OpsItem, the runbook is automatically associated with the related resource of that OpsItem for future reference\. Associated runbooks are ranked higher than others in the **Runbooks** list\.

Use the following procedure to run an Automation runbook that has already been associated with a related resource in an OpsItem\. For information about adding related resources, see [Working with OpsItems](OpsCenter-working-with-OpsItems.md)\.

**To run a resource\-associated runbook to remediate an OpsItem issue**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Open the OpsItem\.

1. In the **Related resources** section, choose the resource on which you want to run the Automation runbook\.

1. Choose **Run automation**, and then choose the associated Automation runbook that you want to run\.

1. Enter the required information for the runbook, and then choose **Execute**\.

1. In the navigation pane, choose **Automation**, and then choose the **Execution ID** link to view the steps and the status of the execution\. 