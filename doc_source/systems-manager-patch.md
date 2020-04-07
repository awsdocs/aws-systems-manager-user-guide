# AWS Systems Manager Patch Manager<a name="systems-manager-patch"></a>

AWS Systems Manager Patch Manager automates the process of patching managed instances with both security related and other types of updates\. You can use Patch Manager to apply patches for both operating systems and applications\. \(On Windows Server, application support is limited to updates for Microsoft applications\.\) You can patch fleets of Amazon EC2 instances or your on\-premises servers and virtual machines \(VMs\) by operating system type\. This includes supported versions of Windows Server, Ubuntu Server, Red Hat Enterprise Linux \(RHEL\), SUSE Linux Enterprise Server \(SLES\), CentOS, Amazon Linux, and Amazon Linux 2\. You can scan instances to see only a report of missing patches, or you can scan and automatically install all missing patches\. 

**Important**  
AWS does not test patches for Windows or Linux before making them available in Patch Manager\.

Patch Manager uses *patch baselines*, which include rules for auto\-approving patches within days of their release, as well as a list of approved and rejected patches\. You can install patches on a regular basis by scheduling patching to run as a Systems Manager maintenance window task\. You can also install patches individually or to large groups of instances by using Amazon EC2 tags\. \(Tags are keys that help identify and sort your resources within your organization\.\) You can add tags to your patch baselines themselves when you create or update them\. 

Patch Manager integrates with AWS Identity and Access Management \(IAM\), AWS CloudTrail, and Amazon CloudWatch Events to provide a secure patching experience that includes event notifications and the ability to audit usage\.

For information about using CloudTrail to monitor Systems Manager actions, see [Logging AWS Systems Manager API Calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)\.

For information about using CloudWatch Events to monitor Systems Manager events, see [Monitoring Systems Manager Events with Amazon CloudWatch Events](monitoring-cloudwatch-events.md)\.

**Getting Started with Patch Manager**  
To get started with Patch Manager, complete the tasks described in the following table\.


| Task | For More Information | 
| --- | --- | 
|  Verify Systems Manager prerequisites  |  [Systems Manager Prerequisites](systems-manager-prereqs.md)  | 
|  Learn how to set up and configure patching  |  [Working with Patch Manager \(Console\)](sysman-patch-working.md)  | 
| Configure permissions for Maintenance Windows\(Required if you intend to use this feature when patching\.\) | [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md) | 
|  Create patch baselines, patch groups, and a maintenance window to run patching in a test environment  |  [Working with Patch Manager \(Console\)](sysman-patch-working.md)  | 

**Topics**
+ [Patch Manager Prerequisites](patch-manager-prerequisites.md)
+ [How Patch Manager Operations Work](patch-manager-how-it-works.md)
+ [About Patching Operations](about-patching-operations.md)
+ [About Patch Baselines](about-patch-baselines.md)
+ [Working with Patch Manager \(Console\)](sysman-patch-working.md)
+ [Tutorial: Patch a Server Environment \(Command Line\)](sysman-patch-cliwalk.md)
+ [AWS CLI Commands for Patch Manager](patch-manager-cli-commands.md)