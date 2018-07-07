# AWS Systems Manager Patch Manager<a name="systems-manager-patch"></a>

AWS Systems Manager Patch Manager automates the process of patching managed instances with security\-related updates\. For Linux\-based instances, you can also install patches for non\-security updates\. You can patch fleets of Amazon EC2 instances or your on\-premises servers and virtual machines \(VMs\) by operating system type\. This includes supported versions of Windows, Ubuntu Server, Red Hat Enterprise Linux \(RHEL\), SUSE Linux Enterprise Server \(SLES\), Amazon Linux, and Amazon Linux 2\. You can scan instances to see only a report of missing patches, or you can scan and automatically install all missing patches\. 

**Important**  
AWS does not test patches for Windows or Linux before making them available in Patch Manager\.

Patch Manager uses patch baselines, which include rules for auto\-approving patches within days of their release, as well as a list of approved and rejected patches\. You can install patches on a regular basis by scheduling patching to run as a Systems Manager Maintenance Window task\. You can also install patches individually or to large groups of instances by using Amazon EC2 tags\. 

Patch Manager integrates with AWS Identity and Access Management \(IAM\), AWS CloudTrail, and Amazon CloudWatch Events to provide a secure patching experience that includes event notifications and the ability to audit usage\.

**Getting Started with Patch Manager**

To get started with Patch Manager, complete the tasks described in the following table\.


| Task | For More Information | 
| --- | --- | 
|  Verify Systems Manager prerequisites  |  [Systems Manager Prerequisites](systems-manager-prereqs.md)  | 
|  Learn how to set up and configure patching  |  [Working with Patch Manager](sysman-patch-working.md)  | 
| Configure permissions for Maintenance Windows\(Required if you intend to use this feature when patching\.\) | [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md) | 
|  Create patch baselines, patch groups, and a Maintenance Window to execute patching in a test environment  |  [Systems Manager Patch Manager Walkthroughs](sysman-patch-walkthrough.md)  | 

**Topics**
+ [Operating Systems Supported by Patch Manager](patch-manager-supported-oses.md)
+ [How Patch Manager Operations Work](patch-manager-how-it-works.md)
+ [Overview of SSM Documents for Patching Instances](patch-manager-ssm-documents.md)
+ [About the SSM Document AWS\-RunPatchBaseline](patch-manager-about-aws-runpatchbaseline.md)
+ [Working with Patch Manager](sysman-patch-working.md)
+ [Systems Manager Patch Manager Walkthroughs](sysman-patch-walkthrough.md)
+ [AWS CLI Commands for Patch Manager](patch-manager-cli-commands.md)