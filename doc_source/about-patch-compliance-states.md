# About Patch Compliance States<a name="about-patch-compliance-states"></a>

After you use Systems Manager Patch Manager to install patches on your instances, compliance status information is immediately available to you in the console or in response to AWS CLI commands or corresponding Systems Manager API actions\.

**Note**  
If you want to assign a specific patch compliance status to an instance, you can use the [put\-compliance\-items](https://docs.aws.amazon.com/cli/latest/reference/ssm/put-compliance-items.html) CLI command or the [PutComplianceItems](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutComplianceItems.html) API action\. Assigning compliance status is not supported in the console\.

## Patch Compliance States<a name="patch-compliance-values"></a>

For all operating systems, the system reports one of the following compliance states for each patch: 
+ **INSTALLED**: Either the patch was already installed, or Patch Manager installed it when the **AWS\-RunPatchBaseline** document was run on the instance\.
+ **INSTALLED\_OTHER**: The patch is not in the baseline, but it is installed on the instance\. An individual might have installed it manually\.
+ **INSTALLED\_REJECTED**: The patch is installed on the instance but is specified in a rejected patches list\. This typically means the patch was installed before it was added to a list of rejected patches\.
+ **INSTALLED\_PENDING\_REBOOT**: The Patch Manager `Install` operation applied the patch to the instance, but the instance has not been rebooted since the patch was applied\. \(Note that patches installed outside of Patch Manager are never given a status of `INSTALLED_PENDING_REBOOT`\.\) This typically means the `NoReboot` option was selected for the `RebootOption` parameter when the **AWS\-RunPatchBaseline** document was last run on the instance\. For more information, see [Parameter name: `RebootOption`](patch-manager-about-aws-runpatchbaseline.md#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.
+ **MISSING**: The patch is approved in the baseline, but it's not installed on the instance\. If you configure the **AWS\-RunPatchBaseline** document task to scan \(instead of install\) the system reports this status for patches that were located during the scan, but have not been installed\.
+ **NOT\_APPLICABLE**: The patch is approved in the baseline, but the service or feature that uses the patch is not installed on the instance\. For example, a patch for Internet Information Services \(IIS\) would show NOT\_APPLICABLE if it was approved in the baseline, but IIS is not installed on the instance\.
**Note**  
This compliance state is only reported on Windows operating systems\.
+ **FAILED**: The patch is approved in the baseline, but it could not be installed\. To troubleshoot this situation, review the command output for information that might help you understand the problem\. 