# AWS\-UpdateWindowsAmi<a name="automation-aws-updatewindowsami"></a>

**Description**

Update a Microsoft Windows Amazon Machine Image \(AMI\)\. By default, this document installs all Windows updates, Amazon software, and Amazon drivers\. It then runs Sysprep to create a new AMI\. Supports Windows Server 2008 R2 or later\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-UpdateWindowsAmi)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ Categories

  Type: String

  Description: \(Optional\) Specify one or more update categories\. You can filter categories using comma\-separated values\. Options: Application, Connectors, CriticalUpdates, DefinitionUpdates, DeveloperKits, Drivers, FeaturePacks, Guidance, Microsoft, SecurityUpdates, ServicePacks, Tools, UpdateRollups, Updates\. Valid formats include a single entry, for example: CriticalUpdates\. Or you can specify a comma separated list: CriticalUpdates,SecurityUpdates\. NOTE: There cannot be any spaces around the commas\.
+ ExcludeKbs

  Type: String

  Description: \(Optional\) Specify one or more Microsoft Knowledge Base \(KB\) article IDs to exclude\. You can exclude multiple IDs using comma\-separated values\. Valid formats: KB9876543 or 9876543\.
+ IamInstanceProfileName

  Type: String

  Default: ManagedInstanceProfile

  Description: \(Required\) The name of the role that enables Systems Manager to manage the instance\.
+ IncludeKbs

  Type: String

  Description: \(Optional\) Specify one or more Microsoft Knowledge Base \(KB\) article IDs to include\. You can install multiple IDs using comma\-separated values\. Valid formats: KB9876543 or 9876543\.
+ InstanceType

  Type: String

  Default: t2\.medium

  Description: \(Optional\) Type of instance to launch as the workspace host\. Instance types vary by region\. Default is t2\.medium\.
+ PostUpdateScript

  Type: String

  Description: \(Optional\) A script provided as a string\. It will run after installing OS updates\.
+ PreUpdateScript

  Type: String

  Description: \(Optional\) A script provided as a string\. It will run prior to installing OS updates\.
+ PublishedDateAfter

  Type: String

  Description: \(Optional\) Specify the date that the updates should be published after\. For example, if 01/01/2017 is specified, any updates that were found during the Windows Update search that have been published on or after 01/01/2017 will be returned\.
+ PublishedDateBefore

  Type: String

  Description: \(Optional\) Specify the date that the updates should be published before\. For example, if 01/01/2017 is specified, any updates that were found during the Windows Update search that have been published on or before 01/01/2017 will be returned\.
+ PublishedDaysOld

  Type: String

  Description: \(Optional\) Specify the amount of days old the updates must be from the published date\. For example, if 10 is specified, any updates that were found during the Windows Update search that have been published 10 or more days ago will be returned\.
+ SeverityLevels

  Type: String

  Description: \(Optional\) Specify one or more MSRC severity levels associated with an update\. You can filter severity levels using comma\-separated values\. By default patches for all security levels are selected\. If value supplied, the update list is filtered by those values\. Options: Critical, Important, Low, Moderate or Unspecified\. Valid formats include a single entry, for example: Critical\. Or, you can specify a comma separated list: Critical,Important,Low\.
+ SourceAmiId

  Type: String

  Description: \(Required\) The source Amazon Machine Image ID\.
+ SubnetId

  Type: String

  Description: \(Optional\) Specify the SubnetId if you want to launch into a specific subnet\.
+ TargetAmiName

  Type: String

  Default: UpdateWindowsAmi\_from\_\{\{SourceAmiId\}\}\_on\_\{\{global:DATE\_TIME\}\}

  Description: \(Optional\) The name of the new AMI that will be created\. Default is a system\-generated string including the source AMI id, and the creation time and date\.