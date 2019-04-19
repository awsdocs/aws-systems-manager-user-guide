# About Patch Compliance States<a name="about-patch-compliance-states"></a>

After you use Systems Manager Patch Manager to install patches on your instances, compliance status information is immediately available to you in the console or in response to AWS CLI commands or corresponding Systems Manager API actions\.

**Note**  
If you want to assign a specific patch compliance status to an instance, you can use the [put\-compliance\-items](https://docs.aws.amazon.com/cli/latest/reference/ssm/put-compliance-items.html) CLI command or the [PutComplianceItems](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutComplianceItems.html) API action\. Assigning compliance status is not supported in the console\.

**Topics**
+ [Patch Compliance States for Ubuntu Server](#patch-compliance-values-ubuntu)
+ [Patch Compliance States for Other Operating Systems](#patch-compliance-values-other)

## Patch Compliance States for Ubuntu Server<a name="patch-compliance-values-ubuntu"></a>

For Ubuntu Server, the rules for package classification into the different compliance states are as follows:
+ **Installed**: Packages that are filtered through the patch baseline, with the candidate version appearing in `trusty-security` \(Ubuntu Server 14\) or `xenial-security` \(Ubuntu Server 16\), and are not upgradable\.
+ **Missing**: Packages that are filtered through the baseline, with the candidate version appearing in `trusty-security` \(Ubuntu Server 14\) or `xenial-security` \(Ubuntu Server 16\), and are upgradable\.
+ **Installed Other**: Packages that are not filtered through the baseline, with the candidate version appearing in `trusty-security` \(Ubuntu Server 14\) or `xenial-security` \(Ubuntu Server 16\), and are not upgradable\. The compliance level for these packages is set to `UNSPECIFIED`\.
+ **NotApplicable**: Packages that are included in [ApprovedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) but are not installed on the system\.
+ **Failed**: Packages that failed to install during the patch operation\.

## Patch Compliance States for Other Operating Systems<a name="patch-compliance-values-other"></a>

For all operating systems besides Ubuntu Server, the system reports one of the following compliance states for each patch: 
+ **INSTALLED**: Either the patch was already installed, or Patch Manager installed it when the **AWS\-RunPatchBaseline** document was run on the instance\.
+ **INSTALLED\_OTHER**: The patch is not in the baseline, but it is installed on the instance\. An individual might have installed it manually\.
+ **INSTALLED\_REJECTED**: The patch is installed on the instance but is specified in a rejected patches list\. This typically means the patch was installed before it was added to a list of rejected patches\.
+ **MISSING**: The patch is approved in the baseline, but it's not installed on the instance\. If you configure the **AWS\-RunPatchBaseline** document task to scan \(instead of install\) the system reports this status for patches that were located during the scan, but have not been installed\.
+ **NOT\_APPLICABLE**: The patch is approved in the baseline, but the service or feature that uses the patch is not installed on the instance\. For example, a patch for a web server service would show NOT\_APPLICABLE if it was approved in the baseline, but the web service is not installed on the instance\.
+ **FAILED**: The patch is approved in the baseline, but it could not be installed\. To troubleshoot this situation, review the command output for information that might help you understand the problem\. 