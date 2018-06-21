# Overview of SSM Documents for Patching Instances<a name="patch-manager-ssm-documents"></a>

This topic describes the seven SSM documents currently available to help you keep your managed instances patched with the latest security\-related updates\. 

We currently recommend using just three of these documents in your patching operations\. Together, these three SSM documents provide you with a full range of patching options using AWS Systems Manager\. Two of these documents were released later than the four legacy SSM documents they replace and represent expansions or consolidations of functionality\.

The three recommended SSM documents include:
+ **AWS\-ConfigureWindowsUpdate**
+ **AWS\-InstallWindowsUpdates**
+ **AWS\-RunPatchBaseline**

The four legacy SSM documents that are still available for use, but might be deprecated in the future, include:
+ **AWS\-ApplyPatchBaseline**
+ **AWS\-FindWindowsUpdates**
+ **AWS\-InstallMissingWindowsUpdates**
+ **AWS\-InstallSpecificWindowsUpdates**

Refer to the following sections for more information about using these SSM documents in your patching operations\.

**Topics**
+ [SSM Documents Recommended for Patching Instances](#patch-manager-ssm-documents-recommended)
+ [Legacy SSM Documents for Patching Instances](#patch-manager-ssm-documents-legacy)

## SSM Documents Recommended for Patching Instances<a name="patch-manager-ssm-documents-recommended"></a>

The following three SSM documents are recommended for use in your managed instance patching operations\.

**Topics**
+ [AWS\-ConfigureWindowsUpdate](#patch-manager-ssm-documents-recommended-AWS-ConfigureWindowsUpdate)
+ [AWS\-InstallWindowsUpdates](#patch-manager-ssm-documents-recommended-AWS-InstallWindowsUpdates)
+ [AWS\-RunPatchBaseline](#patch-manager-ssm-documents-recommended-AWS-RunPatchBaseline)

### AWS\-ConfigureWindowsUpdate<a name="patch-manager-ssm-documents-recommended-AWS-ConfigureWindowsUpdate"></a>

Supports configuring basic Windows Update functions and using them to install updates automatically \(or to disable automatic updates\)\. Available in all AWS Regions\.

This SSM document prompts Windows Update to download and install the specified updates and reboot instances as needed\. Use this document with State Manager to ensure Windows Update maintains its configuration\. You can also run it manually using Run Command to change the Windows Update configuration\. 

The available parameters in this document support specifying a category of updates to install \(or whether to disable automatic updates\), as well as specifying the day of the week and time of day to run patching operations\. This SSM document is most useful if you don't need strict control over Windows updates and don't need to collect compliance information\. 

**Replaces legacy SSM documents: **
+ *None*

### AWS\-InstallWindowsUpdates<a name="patch-manager-ssm-documents-recommended-AWS-InstallWindowsUpdates"></a>

Installs updates on a Windows instance\. Available in all AWS Regions\.

This SSM document provides basic patching functionality in cases where you either want to install a specific update \(using the `Include Kbs` parameter\), or want to install patches with specific classifications or categories but don't need patch compliance information\. 

**Replaces legacy SSM documents:**
+ **AWS\-FindWindowsUpdates**
+ **AWS\-InstallMissingWindowsUpdates**
+ **AWS\-InstallSpecificWindowsUpdates**

The three legacy documents perform different functions, but you can achieve the same results by using different parameter settings with the newer SSM document **AWS\-InstallWindowsUpdates**\. These parameter settings are described in [Legacy SSM Documents for Patching Instances](#patch-manager-ssm-documents-legacy)\.

### AWS\-RunPatchBaseline<a name="patch-manager-ssm-documents-recommended-AWS-RunPatchBaseline"></a>

Installs patches on your instances or scans instances to determine whether any qualified patches are missing\. Available in all AWS Regions\.

**AWS\-RunPatchBaseline** enables you to control patch approvals using patch baselines\. Reports patch compliance information that you can view using the Systems Manager Compliance tools\. These tools provide you with insights on the patch compliance state of your instances, such as which instances are missing patches and what those patches are\. For Linux operating systems, compliance information is provided for patches from both the default source repository configured on an instance and from any alternative source repositories you specify in a custom patch baseline\. For more information about alternative source repositories, see [How to Specify an Alternative Patch Source Repository \(Linux\)](patch-manager-how-it-works-alt-source-repository.md)\. For more information about the Systems Manager Compliance tools, see [AWS Systems Manager Configuration Compliance](systems-manager-compliance.md)\.

 **Replaces legacy documents:**
+ **AWS\-ApplyPatchBaseline**

The legacy document **AWS\-ApplyPatchBaseline** applies only to Windows instances\. The newer **AWS\-RunPatchBaseline** provides the same support for both Windows and Linux systems\. Version 2\.0\.834\.0 or later of SSM Agent is required in order to use the **AWS\-RunPatchBaseline** document\. 

For more information about the **AWS\-RunPatchBaseline** SSM document, see [About the SSM Document AWS\-RunPatchBaseline](patch-manager-about-aws-runpatchbaseline.md)\.

## Legacy SSM Documents for Patching Instances<a name="patch-manager-ssm-documents-legacy"></a>

The following four SSM documents are still available for use in your patching operations\. However, they might be deprecated in the future, so we do not recommend their use\. Instead, use the documents described in [ SSM Documents Recommended for Patching Instances](#patch-manager-ssm-documents-recommended)\.

**Topics**
+ [AWS\-ApplyPatchBaseline](#patch-manager-ssm-documents-legacy-AWS-ApplyPatchBaseline)
+ [AWS\-FindWindowsUpdates](#patch-manager-ssm-documents-legacy-AWS-AWS-FindWindowsUpdates)
+ [AWS\-InstallMissingWindowsUpdates](#patch-manager-ssm-documents-legacy-AWS-InstallMissingWindowsUpdates)
+ [AWS\-InstallSpecificWindowsUpdates](#patch-manager-ssm-documents-legacy-AWS-InstallSpecificWindowsUpdates)

### AWS\-ApplyPatchBaseline<a name="patch-manager-ssm-documents-legacy-AWS-ApplyPatchBaseline"></a>

Supports only Windows instances, but with the same set of parameters found in its replacement, **AWS\-RunPatchBaseline**\. Not available in AWS Regions launched after August 2017\.

**Note**  
The replacement for this SSM document, **AWS\-RunPatchBaseline**, requires version 2\.0\.834\.0 or a later version of SSM Agent\. You can use the **AWS\-UpdateSSMAgent** document to update your instances to the latest version of the agent\. 

### AWS\-FindWindowsUpdates<a name="patch-manager-ssm-documents-legacy-AWS-AWS-FindWindowsUpdates"></a>

Replaced by **AWS\-InstallWindowsUpdates**, which can perform all the same actions\. Not available in AWS Regions launched after April 2017\.

To achieve the same result that you would from this legacy SSM document, use the following parameter configuration with the recommended replacement document, **AWS\-InstallWindowsUpdates**:
+ `Action` = Scan
+ `Allow Reboot` = False

### AWS\-InstallMissingWindowsUpdates<a name="patch-manager-ssm-documents-legacy-AWS-InstallMissingWindowsUpdates"></a>

Replaced by **AWS\-InstallWindowsUpdates**, which can perform all the same actions\. Not available in any AWS Regions launched after April 2017\.

To achieve the same result that you would from this legacy SSM document, use the following parameter configuration with the recommended replacement document, **AWS\-InstallWindowsUpdates**:
+ `Action` = Install
+ `Allow Reboot` = True

### AWS\-InstallSpecificWindowsUpdates<a name="patch-manager-ssm-documents-legacy-AWS-InstallSpecificWindowsUpdates"></a>

Replaced by **AWS\-InstallWindowsUpdates**, which can perform all the same actions\. Not available in any AWS Regions launched after April 2017\.

To achieve the same result that you would from this legacy SSM document, use the following parameter configuration with the recommended replacement document, **AWS\-InstallWindowsUpdates**:
+ `Action` = Install
+ `Allow Reboot` = True
+ `Include Kbs` = *comma\-separated list of KB articles*