# AWS Systems Manager Patch Manager<a name="systems-manager-patch"></a>

Patch Manager, a capability of AWS Systems Manager, automates the process of patching managed nodes with both security\-related updates and other types of updates\.

**Important**  
Beginning December 22, 2022, Systems Manager provides support for *patch policies*, which are the new and recommended method for configuring your patching operations\. Using a single patch policy configuration, you can define patching for all accounts in all Regions in your organization, for only the accounts and Regions you choose, or for a single account\-Region pair\. For more information, see [Introducing patch policies](patch-policies-about.md)\.

You can use Patch Manager to apply patches for both operating systems and applications\. \(On Windows Server, application support is limited to updates for applications released by Microsoft\.\) You can use Patch Manager to install Service Packs on Windows nodes and perform minor version upgrades on Linux nodes\. You can patch fleets of Amazon Elastic Compute Cloud \(Amazon EC2\) instances, edge devices, or your on\-premises servers and virtual machines \(VMs\) by operating system type\. This includes supported versions of several operating systems, as listed in [Patch Manager prerequisites](patch-manager-prerequisites.md)\.\. You can scan instances to see only a report of missing patches, or you can scan and automatically install all missing patches\. To get started with Patch Manager, open the [Systems Manager console](https://console.aws.amazon.com/systems-manager/patch-manager)\. In the navigation pane, choose **Patch Manager**\.

**Note**  
AWS doesn't test patches before making them available in Patch Manager\. Also, Patch Manager doesn't support upgrading major versions of operating systems, such as Windows Server 2016 to Windows Server 2019, or SUSE Linux Enterprise Server \(SLES\) 12\.0 to SLES 15\.0\.  
For Linux\-based operating system types that report a severity level for patches, Patch Manager uses the severity level reported by the software publisher for the update notice or individual patch\. Patch Manager doesn't derive severity levels from third\-party sources, such as the [Common Vulnerability Scoring System](https://www.first.org/cvss/) \(CVSS\), or from metrics released by the [National Vulnerability Database](https://nvd.nist.gov/vuln) \(NVD\)\.

**Patch baselines**  
Patch Manager uses *patch baselines*, which include rules for auto\-approving patches within days of their release, in addition to optional lists of approved and rejected patches\. When a patching operation runs, Patch Manager compares the patches currently applied to a managed node to those that should be applied according to the rules set up in the patch baseline\. You can choose for Patch Manager to show you only a report of missing patches \(a `Scan` operation\), or you can choose for Patch Manager to automatically install all patches it find are missing from a managed node \(a `Scan and install` operation\)\.

**Patching operation methods**  
Patch Manager currently offers four methods for running `Scan` and `Scan and install` operations:
+ **\(Recommended\) A patch policy configured in Quick Setup** – Based on integration with AWS Organizations, a single patch policy can define patching schedules and patch baselines for an entire organization, including multiple AWS accounts and all AWS Regions those accounts operate in\. A patch policy can also target only some organizational units \(OUs\) in an organization\. You can use a single patch policy to scan and install on different schedules\. For more information, see [Automate organization\-wide patching using a Quick Setup patch policyAutomate organization\-wide patching](quick-setup-patch-manager.md) and [Introducing patch policies](patch-policies-about.md)\.
+ **A Host Management option configured in Quick Setup** – Host Management configurations are also supported by integration with AWS Organizations, making it possible to run a patching operation for up to an entire Organization\. However, this option is limited to scanning for missing patches using the current default patch baseline and providing results in compliance reports\. This operation method can't install patches\. For more information, see [Quick Setup Host Management](quick-setup-host-management.md)\.
+ **A maintenance window to run a patch `Scan` or `Install` task** – A maintenance window, which you set up in the Systems Manager capability called Maintenance Windows, can be configured to run different types of tasks on a schedule you define\. A Run Command\-type task can bed used to run `Scan` or `Scan and install` tasks a set of managed nodes that you choose\. Each maintenance window task can target managed nodes in only a single AWS account\-AWS Region pair\. For more information, see [Walkthrough: Creating a maintenance window for patching \(console\)](sysman-patch-mw-console.md)\.
+ **An on\-demand 'Patch now' operation in Patch Manager** – The **Patch now** option lets you bypass schedule setups when you need to patch managed nodes as quickly as possible\. Using **Patch now**, you specify whether to run `Scan` or `Scan and install` operation and which managed nodes to run the operation on\. You can also choose to running Systems Manager documents \(SSM documents\) as lifecycle hooks during patching the patching operation\. Each **Patch now** operation can target managed nodes in only a single AWS account\-AWS Region pair\. For more information, see [Patching managed nodes on demand](patch-on-demand.md)\.

**Compliance reporting**  
After a `Scan` operation, you can use the Systems Manager console to view information about which of your managed nodes are out of patch compliance, and which patches are missing from each of those nodes\. You can also generate patch compliance reports in \.csv format that are sent to an Amazon Simple Storage Service \(Amazon S3\) bucket of your choice\. You can generate one\-time reports, or generate reports on a regular schedule\. For a single managed node, reports include details of all patches for the node\. For a report on all managed nodes, only a summary of how many patches are missing is provided\. After a report is generated, you can use a tool like Amazon QuickSight to import and analyze the data\. For more information, see [Working with patch compliance reports](patch-compliance-reports.md)\.

**Note**  
A compliance item generated through the use of a patch policy has an execution type of `PatchPolicy`\. A compliance item not generated in a patch policy operation has an execution type of `Command`\.

**Integrations**  
Patch Manager integrates with the following other AWS services: 
+ **AWS Identity and Access Management \(IAM\)** – Use IAM to control which users, groups, and roles have access to Patch Manager operations\. For more information, see [How AWS Systems Manager works with IAM](security_iam_service-with-iam.md) and [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. 
+ **AWS CloudTrail** – Use CloudTrail to record an auditable history of patching operation events initiated by users, roles, or groups\. For more information, see [Logging AWS Systems Manager API calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)\.
+ **AWS Security Hub** – Patch compliance data from Patch Manager can be sent to AWS Security Hub\. Security Hub gives you a comprehensive view of your high\-priority security alerts and compliance status\. It also monitors the patching status of your fleet\. For more information, see [Integrating Patch Manager with AWS Security Hub](patch-manager-security-hub-integration.md)\. 
+ **AWS Config** – Set up recording in AWS Config to view Amazon EC2 instance management data in the Patch Manager Dashboard\. For more information, see [Viewing patch Dashboard summaries \(console\)](view-patch-dashboard-summaries.md)\.

**Topics**
+ [Introducing patch policies](patch-policies-about.md)
+ [Patch Manager prerequisites](patch-manager-prerequisites.md)
+ [How Patch Manager operations work](patch-manager-how-it-works.md)
+ [About SSM documents for patching managed nodes](patch-manager-ssm-documents.md)
+ [About patch baselines](about-patch-baselines.md)
+ [Using Kernel Live Patching on Amazon Linux 2 managed nodes](kernel-live-patching.md)
+ [Working with Patch Manager \(console\)](sysman-patch-working.md)
+ [Working with Patch Manager \(AWS CLI\)](patch-manager-cli-commands.md)
+ [AWS Systems ManagerPatch Manager walkthroughs](patch-walkthroughs.md)
+ [Troubleshooting Patch Manager](patch-manager-troubleshooting.md)