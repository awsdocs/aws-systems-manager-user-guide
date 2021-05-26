# AWS Systems Manager Patch Manager<a name="systems-manager-patch"></a>

Patch Manager, a capability of AWS Systems Manager, automates the process of patching managed instances with both security related and other types of updates\. You can use Patch Manager to apply patches for both operating systems and applications\. \(On Windows Server, application support is limited to updates for Microsoft applications\.\) You can use Patch Manager to install Service Packs on Windows instances and perform minor version upgrades on Linux instances\. You can patch fleets of Amazon Elastic Compute Cloud \(Amazon EC2\) instances or your on\-premises servers and virtual machines \(VMs\) by operating system type\. This includes supported versions of Amazon Linux, Amazon Linux 2, CentOS, Debian Server, macOS, Oracle Linux, Red Hat Enterprise Linux \(RHEL\), SUSE Linux Enterprise Server \(SLES\), Ubuntu Server, and Windows Server\. You can scan instances to see only a report of missing patches, or you can scan and automatically install all missing patches\. 

**Important**  
AWS does not test patches for Windows Server or Linux before making them available in Patch Manager\. Also, Patch Manager doesn't support upgrading major versions of operating systems, such as Windows Server 2016 to Windows Server 2019, or SUSE Linux Enterprise Server \(SLES\) 12\.0 to SLES 15\.0\.

Patch Manager uses *patch baselines*, which include rules for auto\-approving patches within days of their release, as well as a list of approved and rejected patches\. You can install patches on a regular basis by scheduling patching to run as a Systems Manager maintenance window task\. You can also install patches individually or to large groups of instances by using Amazon EC2 tags\. \(Tags are keys that help identify and sort your resources within your organization\.\) You can add tags to your patch baselines themselves when you create or update them\. 

Patch Manager provides options to scan your instances and report compliance on a schedule, install available patches on a schedule, and patch or scan instances on demand whenever you need to\. You can also generate patch compliance reports that are sent to an Amazon Simple Storage Service \(Amazon S3\) bucket of your choice\. You can generate one\-time reports, or generate reports on a regular schedule\. For a single instance, reports include details of all patches for the instance\. For a report on all instances, only a summary of how many patches are missing is provided\.

Patch Manager integrates with AWS Identity and Access Management \(IAM\), AWS CloudTrail, and Amazon EventBridge to provide a secure patching experience that includes event notifications and the ability to audit usage\.

For information about using CloudTrail to monitor Systems Manager actions, see [Logging AWS Systems Manager API calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)\.

For information about using EventBridge to monitor Systems Manager events, see [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md)\.

**Topics**
+ [Patch Manager prerequisites](patch-manager-prerequisites.md)
+ [How Patch Manager operations work](patch-manager-how-it-works.md)
+ [About patching operations](about-patching-operations.md)
+ [About patch baselines](about-patch-baselines.md)
+ [Remediating out\-of\-compliance instances with Patch Manager](patch-compliance-remediation.md)
+ [Working with Patch Manager \(console\)](sysman-patch-working.md)
+ [Use Kernel Live Patching on Amazon Linux 2 instances](kernel-live-patching.md)
+ [Integration with AWS Security Hub](security-hub-integration.md)
+ [Working with Patch Manager \(AWS CLI\)](patch-manager-cli-commands.md)
+ [AWS Systems Manager Patch Manager walkthroughs](patch-walkthroughs.md)
+ [Troubleshooting Patch Manager](patch-manager-troubleshooting.md)