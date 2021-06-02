# Generating \.csv patch compliance reports \(console\)<a name="patch-compliance-reports-to-s3"></a>

You can use the AWS Systems Manager console to generate patch compliance reports that are saved as a \.csv file to an Amazon Simple Storage Service \(Amazon S3\) bucket of your choice\. You can generate a single on\-demand report or specify a schedule for generating the reports automatically\. 

Reports can be generated for a single instance or for all instances in your AWS account\. For a single instance, a report contains comprehensive details, including the IDs of patches related to an instance being noncompliant\. For a report on all instances, only summary information and counts of noncompliant instances' patches are provided\.

**Note**  
After a report is generated, you can use a tool like Amazon QuickSight to import and analyze the data\. Amazon QuickSight is a business intelligence \(BI\) service you can use to explore and interpret information in an interactive visual environment\. For more information, see the *[Amazon QuickSight User Guide](https://docs.aws.amazon.com/quicksight/latest/user/)*\.

You can also specify an Amazon Simple Notification Service \(Amazon SNS\) topic to use for sending notifications when a report is generated\.

**Service roles for generating patch compliance reports**  
The first time you generate a report, Systems Manager creates a service role named `AWS-SystemsManager-PatchSummaryExportRole` to use for the export process\. The first time you generate a report on a schedule, Systems Manager creates another service role named `AWS-EventBridge-Start-SSMAutomationRole`, along with the service role `AWS-SystemsManager-PatchSummaryExportRole` \(if not created already\) to use for the export process\. `AWS-EventBridge-Start-SSMAutomationRole` lets Amazon EventBridge start an automation using the runbook [AWS\-ExportPatchReportToS3 ](https://docs.aws.amazon.com/systems-manager-automation-runbooks/latest/userguide/automation-aws-exportpatchreporttos3)\.

We recommend against attempting to modify these policies and roles\. Doing so could cause patch compliance report generation to fail\. For more information, see [Troubleshooting patch compliance report generation](#patch-compliance-reports-troubleshooting)\.

**Topics**
+ [What's in a generated patch compliance report?](#patch-compliance-reports-to-s3-examples)
+ [Generating patch compliance reports for a single instance](#patch-compliance-reports-to-s3-one-instance)
+ [Generating patch compliance reports for all instances](#patch-compliance-reports-to-s3-all-instances)
+ [Viewing patch compliance reporting history](#patch-compliance-reporting-history)
+ [Viewing patch compliance reporting schedules](#patch-compliance-reporting-schedules)
+ [Troubleshooting patch compliance report generation](#patch-compliance-reports-troubleshooting)

## What's in a generated patch compliance report?<a name="patch-compliance-reports-to-s3-examples"></a>

This topic provides information about the types of content included in the patch compliance reports that are generated and downloaded to a specified S3 bucket\.

### Report format for a single instance<a name="patch-compliance-reports-to-s3-examples-single-instance"></a>

A report generated for a single instance provides both summary and detailed information\.

[Download a sample report \(single instance\)](samples/Sample-single-instance-patch-compliance-report.zip)

Summary information for a single instance includes the following:
+ Index
+ Instance ID
+ Instance name
+ Instance IP
+ Platform name
+ Platform version
+ SSM Agent version
+ Patch baseline
+ Patch group
+ Compliance status
+ Compliance severity
+ Noncompliant Critical severity patch count
+ Noncompliant High severity patch count
+ Noncompliant Medium severity patch count
+ Noncompliant Low severity patch count
+ Noncompliant Informational severity patch count
+ Noncompliant Unspecified severity patch count

Detailed information for a single instance includes the following:
+ Index
+ Instance ID
+ Instance name
+ Patch name
+ KB ID/Patch ID
+ Patch state
+ Last report time
+ Compliance level
+ Patch severity
+ Patch classification
+ CVE ID
+ Patch baseline
+ Logs URL
+ Instance IP
+ Platform name
+ Platform version
+ SSM Agent version

### Report format for all instances<a name="patch-compliance-reports-to-s3-examples-all-instances"></a>

A report generated for all instances provides only summary information\.

[Download a sample report \(all instances\)](samples/Sample-all-instances-patch-compliance-report.zip)

Summary information for all instances includes the following:
+ Index
+ Instance ID
+ Instance name
+ Instance IP
+ Platform name
+ Platform version
+ SSM Agent version
+ Patch baseline
+ Patch group
+ Compliance status
+ Compliance severity
+ Noncompliant Critical severity patch count
+ Noncompliant High severity patch count
+ Noncompliant Medium severity patch count
+ Noncompliant Low severity patch count
+ Noncompliant Informational severity patch count
+ Noncompliant Unspecified severity patch count

## Generating patch compliance reports for a single instance<a name="patch-compliance-reports-to-s3-one-instance"></a>

Use the following procedure to generate a patch summary report for a single instance in your AWS account\. The report for a single instance provides details about each patch that is out of compliance, including patch names and IDs\. 

**To generate patch compliance reports for a single instance**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose the **Reporting** tab\.

1. Choose the button for the row of the instance for which you want to generate a report, and then choose **View detail**\.

1. On the **Patch summary page**, choose **Export to S3**\.

1. For **Report name**, enter a name to help you identify the report later\.

1. For **Reporting frequency**, choose one of the following:
   + **On demand** – Create a one\-time report\. Skip to Step 8\.
   + **On a schedule** – Specify a recurring schedule for automatically generating reports\. Continue to Step 7\.

1. For **Schedule type**, specify either a rate expression, such as every 3 days, or provide a cron expression to set the report frequency\.

   For information about cron expressions, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

1. For **Bucket name**, select the name of an S3 bucket where you want to store the \.csv report files\.
**Important**  
If you are working in an AWS Region that was launched after March 20, 2019, you must select an S3 bucket in that same Region\. Regions launched after that date were disabled by default\. For more information and a list of these Regions, see [Enabling a Region](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html#rande-manage-enable) in the *Amazon Web Services General Reference*\.

1. \(Optional\) To send notifications when the report is generated, choose an existing Amazon SNS topic from **SNS topic Amazon Resource Name \(ARN\)**\.

1. Choose **Submit**\.

For information about viewing a history of generated reports, see [Viewing patch compliance reporting history](#patch-compliance-reporting-history)\.

For information about viewing details of reporting schedules you have created, see [Viewing patch compliance reporting schedules](#patch-compliance-reporting-schedules)\.

## Generating patch compliance reports for all instances<a name="patch-compliance-reports-to-s3-all-instances"></a>

Use the following procedure to generate a patch summary report for all instances in your AWS account\. The report for all instances indicates which instances are out of compliance and the numbers of out\-of\-compliance patches\. It does not provide the names or other identifiers of the patches\. For these additional details, you can generate a patch compliance report for a single instance\. For information, see [Generating patch compliance reports for a single instance](#patch-compliance-reports-to-s3-one-instance) earlier in this topic\. 

**To generate patch compliance reports for all instances**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose the **Reporting** tab\.

1. Choose **Export to S3**\. Do not select an instance ID\.

1. For **Report name**, enter a name to help you identify the report later\.

1. For **Reporting frequency**, choose one of the following:
   + **On demand** – Create a one\-time report\. Skip to Step 8\.
   + **On a schedule** – Specify a recurring schedule for automatically generating reports\. Continue to Step 7\.

1. For **Schedule type**, specify either a rate expression, such as every 3 days, or provide a cron expression to set the report frequency\.

   For information about cron expressions, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

1. For **Bucket name**, select the name of an S3 bucket where you want to store the \.csv report files\.
**Important**  
If you are working in an AWS Region that was launched after March 20, 2019, you must select an S3 bucket in that same Region\. Regions launched after that date were disabled by default\. For more information and a list of these Regions, see [Enabling a Region](https://docs.aws.amazon.com/general/latest/gr/rande-manage.html#rande-manage-enable) in the *Amazon Web Services General Reference*\.

1. \(Optional\) To send notifications when report the report is generated, choose an existing Amazon SNS topic from **SNS topic Amazon Resource Name \(ARN\)**\.

1. Choose **Submit**\.

For information about viewing a history of generated reports, see [Viewing patch compliance reporting history](#patch-compliance-reporting-history)\.

For information about viewing details of reporting schedules you have created, see [Viewing patch compliance reporting schedules](#patch-compliance-reporting-schedules)\.

## Viewing patch compliance reporting history<a name="patch-compliance-reporting-history"></a>

Use the information in this topic to help you view details about the patch compliance reports generated in your AWS account\.

**To view patch compliance reporting history**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose the **Reporting** tab\.

1. Choose **View all S3 exports**, and then choose the **Export history** tab\.

## Viewing patch compliance reporting schedules<a name="patch-compliance-reporting-schedules"></a>

Use the information in this topic to help you view details about the patch compliance reporting schedules created in your AWS account\.

**To view patch compliance reporting history**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose the **Reporting** tab\.

1. Choose **View all S3 exports**, and then choose the **Scheduled report** tab\.

## Troubleshooting patch compliance report generation<a name="patch-compliance-reports-troubleshooting"></a>

Use the following information to help you troubleshoot problems with generating patch compliance report generation in Patch Manager, a capability of AWS Systems Manager\.

**Topics**
+ [A message reports that the `AWS-SystemsManager-PatchSummaryExportRolePolicy` policy is corrupted](#patch-compliance-reports-troubleshooting-1)
+ [After deleting patch compliance policies or roles, scheduled reports are not generated successfully](#patch-compliance-reports-troubleshooting-2)

### A message reports that the `AWS-SystemsManager-PatchSummaryExportRolePolicy` policy is corrupted<a name="patch-compliance-reports-troubleshooting-1"></a>

**Problem**: You receive an error message similar to the following, indicating the `AWS-SystemsManager-PatchSummaryExportRolePolicy` is corrupted:

```
An error occurred while updating the AWS-SystemsManager-PatchSummaryExportRolePolicy
policy. If you have edited the policy, you might need to delete the policy, and any 
role that uses it, then try again. Systems Manager recreates the roles and policies 
you have deleted.
```
+ **Solution**: Perform the following steps:

  1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

  1. Do one of the following:

     **On\-demand reports** – If the problem occurred while generating a one\-time on\-demand report, in the left navigation, choose **Policies**, search for `AWS-SystemsManager-PatchSummaryExportRolePolicy`, then delete the policy\. Next, choose **Roles**, search for `AWS-SystemsManager-PatchSummaryExportRole`, then delete the role\.

     **Scheduled reports** – If the report occurred while generating a report on a schedule, in the left navigation, choose **Policies**, search one at a time for `AWS-EventBridge-Start-SSMAutomationRolePolicy` and `AWS-SystemsManager-PatchSummaryExportRolePolicy`, and delete each policy\. Next, choose **Roles**, search one at a time for `AWS-EventBridge-Start-SSMAutomationRole` and `AWS-SystemsManager-PatchSummaryExportRole`, and delete each role\.

  1. Follow the steps to generate or schedule a new patch compliance report\.

### After deleting patch compliance policies or roles, scheduled reports are not generated successfully<a name="patch-compliance-reports-troubleshooting-2"></a>

**Problem**: The first time you generate a report, Systems Manager creates a service role and a policy to use for the export process \(`AWS-SystemsManager-PatchSummaryExportRole` and `AWS-SystemsManager-PatchSummaryExportRolePolicy`\)\. The first time you generate a report on a schedule, Systems Manager creates another service role and a policy \(`AWS-EventBridge-Start-SSMAutomationRole` and `AWS-EventBridge-Start-SSMAutomationRolePolicy`\)\. These let Amazon EventBridge start an automation using the runbook [AWS\-ExportPatchReportToS3 ](https://docs.aws.amazon.com/systems-manager-automation-runbooks/latest/userguide/automation-aws-exportpatchreporttos3)\.

If you delete any of these policies or roles, the connections between your schedule and your specified S3 bucket and Amazon SNS topic might be lost\. 
+ **Solution**: To work around this problem, we recommend deleting the previous schedule and creating a new schedule to replace the one that was experiencing issues\.