# AWSPremiumSupport\-TroubleshootEC2DiskUsage<a name="automation-awspremiumsupport-troubleshootEC2diskusage"></a>

 **Description** 

The AWSPremiumSupport\-TroubleshootEC2DiskUsage runbook helps you investigate and potentially remediate issues with Amazon Elastic Compute Cloud \(Amazon EC2\) instance root and non\-root disk usage\. If possible, the runbook attempts to remediate issues by extending the volume and its file system\. To perform these tasks, this runbook orchestrates the execution of several runbooks based on the operating system of the affected instance\.

The first runbook, AWSPremiumSupport\-DiagnoseDiskUsageOnWindows or AWSPremiumSupport\-DiagnoseDiskUsageOnLinux, determines if disk issues can be mitigated by expanding the volume\.

The second runbook, AWSPremiumSupport\-ExtendVolumesOnWindows or AWSPremiumSupport\-ExtendVolumesOnLinux, uses the output of the first runbook to run Python code that modifies the volume\. After the volume has been modified, the runbook extends the partition and file system of the affected volumes\.

**Important**  
Access to **AWSPremiumSupport\-\*** runbooks requires an Enterprise or Business Support Subscription\. For more information, see [Compare AWS Support Plans](https://aws.amazon.com/premiumsupport/plans/)\.

This document was built in collaboration with AWS Managed Services \(AMS\)\. AMS helps you manage your AWS infrastructure more efficiently and securely\. AMS also provides operational flexibility, enhanced security and compliance, capacity optimization, and cost\-savings identification\. For more information, see [AWS Managed Services](https://aws.amazon.com/managed-services/)\. 

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSPremiumSupport-TroubleshootEC2DiskUsage)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, Windows

**Parameters**
+ InstanceId

  Type: String

  Allowed values: ^i\-\[a\-z0\-9\]\{8,17\}$

  Description: \(Required\) ID of your Amazon EC2 instance\.
+ VolumeExpansionEnabled

  Type: Boolean

  Description: \(Optional\) Flag to control whether the document will extend the volumes and partitions affected\.

  Default: true
+ VolumeExpansionUsageTrigger

  Type: String

  Description: \(Optional\) Minimum usage of partition space required to trigger extension \(in percentage\)\.

  Allowed values: ^\[0\-9\]\{1,2\}$

   Default: 85
+ VolumeExpansionCapSize

  Type: String

  Description: \(Optional\) Maximum size that the Amazon Elastic Block Store \(Amazon EBS\) volume will be increased to \(in GiB\)\.

  Allowed values: ^\[0\-9\]\{1,4\}$

  Default: 2048
+ VolumeExpansionGibIncrease

  Type: String

  Description: \(Optional\) Increase in GiB of the volume\. The biggest net increase between VolumeExpansionGibIncrease and VolumeExpansionPercentageIncrease will be used\.

  Allowed values: ^\[0\-9\]\{1,4\}$

  Default: 20
+ VolumeExpansionPercentageIncrease

  Type: String

  Description: \(Optional\) Increase in percentage of the volume\. The biggest net increase between VolumeExpansionGibIncrease and VolumeExpansionPercentageIncrease will be used\.

  Allowed values: ^\[0\-9\]\{1,2\}$

  Default: 20
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this runbook\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ec2:DescribeVolumes`
+ `ec2:DescribeVolumesModifications`
+ `ec2:ModifyVolume`
+ `ec2:DescribeInstances`
+ `ec2:CreateImage`
+ `ec2:DescribeImages`
+ `ec2:DescribeTags`
+ `ec2:CreateTags`
+ `ec2:DeleteTags`
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ssm:DescribeAutomationStepExecutions`
+ `ssm:DescribeAutomationExecutions`
+ `ssm:SendCommand`
+ `ssm:DescribeInstanceInformation`
+ `ssm:ListCommands`
+ `ssm:ListCommandInvocations`

 **Document Steps** 

1. aws:assertAwsResourceProperty \- Check if the instance is a managed instance

1. aws:executeAwsApi \- Describe the input instance to get the platform\.

1. aws:branch \- Check the instance's platform

   1. \(Platform is windows\) If the input instance is Windows:

      1. aws:executeAutomation \- Execute SSM document AWSPremiumSupport\-DiagnoseDiskUsageOnWindows in order to diagnose disk usage issues on the instance\.

      1. aws:executeAwsApi \- Get the output the previous SSM execution

      1. aws:branch \- Check if, based on the output of the diagnostics, there are volumes that can be expanded to mitigate the alert\.

         1. There are no volumes that need to be expanded: End the execution

         1. There are volumes that need to be expanded:

            1. aws:executeAwsApi \- Create Image of the Instance

            1. aws:waitForAwsResourceProperty \- Wait for Image to be Available

            1. aws:executeAutomation \- Execute SSM document AWSPremiumSupport\-ExtendVolumesOnWindows in order to perform the volume modification as well as the required steps in the OS to make the new space available\.

   1. \(Platform is not windows\) If the input instance is not Windows:

      1. aws:executeAutomation \- Execute SSM document AWSPremiumSupport\-DiagnoseDiskUsageOnLinux in order to diagnose disk usage issues on the instance\.

      1. aws:executeAwsApi \- Get the output the previous SSM execution

      1. aws:branch \- Check if, based on the output of the diagnostics, there are volumes that can be expanded to mitigate the alert\.

         1. There are no volumes that need to be expanded: End the execution

         1. There are volumes that need to be expanded:

            1. aws:executeAwsApi \- Create Image of the Instance

            1. aws:waitForAwsResourceProperty \- Wait for Image to be Available

            1. aws:executeAutomation \- Execute SSM document AWSPremiumSupport\-ExtendVolumesOnLinux in order to perform the volume modification as well as the required steps in the OS to make the new space available\.

 **Outputs** 

diagnoseDiskUsageAlertOnWindows\.Output

extendVolumesOnWindows\.Output

diagnoseDiskUsageAlertOnLinux\.Output

extendVolumesOnLinux\.Output

BackupAMILinux\.ImageId

BackupAMIWindows\.ImageId 