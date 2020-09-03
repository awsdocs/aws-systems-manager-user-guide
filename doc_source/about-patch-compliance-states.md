# About patch compliance status values<a name="about-patch-compliance-states"></a>

After you use Systems Manager Patch Manager to install patches on your instances, compliance status information is immediately available to you in the console or in response to AWS CLI commands or corresponding Systems Manager API actions\.

**Note**  
If you want to assign a specific patch compliance status to an instance, you can use the [put\-compliance\-items](https://docs.aws.amazon.com/cli/latest/reference/ssm/put-compliance-items.html) CLI command or the [PutComplianceItems](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutComplianceItems.html) API action\. Assigning compliance status is not supported in the console\.

In the console, you can view patch compliance data in the following Systems Manager capabilities:
+ [Explorer](Explorer.md)
+ [Compliance](systems-manager-compliance.md)
+ [Managed Instances](managed_instances.md)

Using the AWS CLI, you can view summary information about patches on an instance by running commands such as the following:
+ [describe\-instance\-patch\-states](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-instance-patch-states.html)
+ [describe\-instance\-patch\-states\-for\-patch\-group](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-instance-patch-states-for-patch-group.html)
+ [describe\-patch\-group\-state](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-patch-group-state.html)

## Patch compliance values for Debian and Ubuntu Server<a name="patch-compliance-values-ubuntu"></a>

For Debian and Ubuntu Server, the rules for package classification into the different compliance states are described in the following table\.

**Note**  
Keep the following in mind when you are evaluating the **Installed**, **Installed Other**, and **Missing** status values: If you do not select the **Include nonsecurity updates** check box when creating or updating a patch baseline, patch candidate versions are limited to patches included in `trusty-security` \(Ubuntu Server 14\), `xenial-security` \(Ubuntu Server 16\), `bionic-security` \(Ubuntu Server 18\), `focal-security` \(Ubuntu Server 20\), or `debian-security` \(Debian Stretch or Jessie\)\. If you do select the **Include nonsecurity updates** check box, patches from other repositories are considered as well\.


|  Patch status | Description | Compliance status | 
| --- | --- | --- | 
| Installed |  The patch is listed in the patch baseline and is installed on the instance\. It could have been installed either manually by an individual or automatically by Patch Manager when the `AWS-RunPatchBaseline` document was run on the instance\.  | Compliant | 
| Installed Other |  The patch is not included in the baseline or is not approved by the baseline but is installed on the instance\. The patch might have been installed manually, the package could be a required dependency of another approved patch, or the patch might have been included in an InstallOverrideList operation\. If you do not specify `Block` as the **Rejected patches** action, `Installed_Other` patches also includes installed but rejected patches\.   | Compliant | 
| Installed Pending Reboot |  The Patch Manager `Install` operation applied the patch to the instance, but the instance has not been rebooted since the patch was applied\. \(Note that patches installed outside of Patch Manager are never given a status of `INSTALLED_PENDING_REBOOT`\.\) This typically means the `NoReboot` option was selected for the `RebootOption` parameter when the **AWS\-RunPatchBaseline** document was last run on the instance\. For more information, see [Parameter name: `RebootOption`](patch-manager-about-aws-runpatchbaseline.md#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.  | Non\-Compliant | 
| Installed Rejected |  The patch is installed on the instance but is specified in a **Rejected patches** list\. This typically means the patch was installed before it was added to a list of rejected patches\.  | Non\-Compliant | 
| Missing |  Packages that are filtered through the baseline and not already installed\.  | Non\-Compliant | 
| Failed |  Packages that failed to install during the patch operation\.  | Non\-Compliant | 

## Patch compliance values for other operating systems<a name="patch-compliance-values"></a>

For all operating systems besides Debian and Ubuntu Server, the rules for package classification into the different compliance states are described in the following table\. 


|  Patch status | Description | Compliance value | 
| --- | --- | --- | 
| INSTALLED |  The patch is listed in the patch baseline and is installed on the instance\. It could have been installed either manually by an individual or automatically by Patch Manager when the **AWS\-RunPatchBaseline** document was run on the instance\.  | Compliant | 
| INSTALLED\_OTHER |  The patch is not in the baseline, but it is installed on the instance\. The patch might have been installed manually, or the package could be a required dependency of another approved patch\. If you do not specify `Block` as the **Rejected patches** action, `Installed_Other` patches also includes installed but rejected patches\.  | Compliant | 
| INSTALLED\_REJECTED |  The patch is installed on the instance but is specified in a rejected patches list\. This typically means the patch was installed before it was added to a list of rejected patches\.  | Non\-Compliant | 
| INSTALLED\_PENDING\_REBOOT |  The Patch Manager `Install` operation applied the patch to the instance \(or a patch was applied to a Windows Server instance outside of Patch Manager\), but the instance has not been rebooted since the patch was applied\. \(Note that patches installed outside of Patch Manager are never given a status of `INSTALLED_PENDING_REBOOT`\.\) This typically means the `NoReboot` option was selected for the `RebootOption` parameter when the **AWS\-RunPatchBaseline** document was last run on the instance\. For more information, see [Parameter name: `RebootOption`](patch-manager-about-aws-runpatchbaseline.md#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.  | Non\-Compliant | 
| MISSING |  The patch is approved in the baseline, but it's not installed on the instance\. If you configure the **AWS\-RunPatchBaseline** document task to scan \(instead of install\), the system reports this status for patches that were located during the scan but have not been installed\.  | Non\-Compliant | 
| NOT\_APPLICABLE |  The patch is approved in the baseline, but the service or feature that uses the patch is not installed on the instance\. For example, a patch for web server service such as Internet Information Services \(IIS\) would show NOT\_APPLICABLE if it was approved in the baseline, but the web service is not installed on the instance\. A patch can also be marked NOT\_APPLICABLE if it has been superseded by a subsequent update\. This means that the later update is installed and the NOT\_APPLICABLE update is no longer required\.  This compliance state is only reported on Windows Server operating systems\.   | Not applicable | 
| FAILED |  The patch is approved in the baseline, but it could not be installed\. To troubleshoot this situation, review the command output for information that might help you understand the problem\.  | Non\-Compliant | 