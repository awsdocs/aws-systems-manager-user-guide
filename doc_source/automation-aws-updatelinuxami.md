# AWS\-UpdateLinuxAmi<a name="automation-aws-updatelinuxami"></a>

**Description**

Update an Amazon Machine Image \(AMI\) with Linux distribution packages and Amazon software\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-UpdateLinuxAmi)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Default: arn:aws:iam::\{\{global:ACCOUNT\_ID\}\}:role/AutomationServiceRole

  Description: \(Required\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ ExcludePackages

  Type: String

  Default: none

  Description: \(Optional\) Names of packages to hold back from updates, under all conditions\. By default \("none"\), no package is excluded\.
+ IamInstanceProfileName

  Type: String

  Default: ManagedInstanceProfile

  Description: \(Required\) The instance profile that enables Systems Manager to manage the instance\.
+ IncludePackages

  Type: String

  Default: all

  Description: \(Optional\) Only update these named packages\. By default \("all"\), all available updates are applied\.
+ InstanceType

  Type: String

  Default: t2\.micro

  Description: \(Optional\) Type of instance to launch as the workspace host\. Instance types vary by Region\.
+ PostUpdateScript

  Type: String

  Default: none

  Description: \(Optional\) URL of a script to run after package updates are applied\. Default \("none"\) is to not run a script\.
+ PreUpdateScript

  Type: String

  Default: none

  Description: \(Optional\) URL of a script to run before updates are applied\. Default \("none"\) is to not run a script\.
+ SourceAmiId

  Type: String

  Description: \(Required\) The source Amazon Machine Image ID\.
+ TargetAmiName

  Type: String

  Default: UpdateLinuxAmi\_from\_\{\{SourceAmiId\}\}\_on\_\{\{global:DATE\_TIME\}\}

  Description: \(Optional\) The name of the new AMI that will be created\. Default is a system\-generated string including the source AMI id, and the creation time and date\.