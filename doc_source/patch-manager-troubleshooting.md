# Troubleshooting Patch Manager<a name="patch-manager-troubleshooting"></a>

Use the following information to help you troubleshoot problems with Patch Manager\.

## Troubleshooting mismatched product family/product pairs<a name="patch-manager-troubleshooting-product-family-mismatch"></a>

**Problem**: When you create a patch baseline in the console, you specify a product family and a product\. For example, you might choose:
+ **Product family**: **Office**

  **Product**: **Office 2016**

**Cause**: If you attempt to create a patch baseline with a mismatched product family/product pair, an error message is displayed\. The following are reasons this can occur:
+ You selected a valid product family/product pair but then removed the product family selection\.
+ You chose a product from the **Obsolete or mismatched options** sublist instead of the **Available and matching options** sublist\. 

  Items in the product **Obsolete or mismatched options** sublist might have been entered in error through an SDK or AWS Command Line Interface \(AWS CLI\) `create-patch-baseline` command\. This could mean a typo was introduced or a product was assigned to the wrong product family\. A product also appears in the **Obsolete or mismatched options** sublist if it was specified for a previous patch baseline but currently has no patches available from Microsoft\. 

**Solution**: To avoid this issue in the console, always choose options from the **Currently available options** sublists\.

You can also view the products that have currently available patches by using the `[describe\-patch\-properties](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-patch-properties.html)` command in the AWS CLI or the `[DescribePatchProperties](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribePatchProperties.html)` API command\.

## Troubleshooting `AWS-RunPatchBaseline` output returns an `HRESULT`<a name="patch-manager-troubleshooting-hresult"></a>

**Problem**: You received an error like the following\.

```
----------ERROR-------
Invoke-PatchBaselineOperation : Exception Details: An error occurred when 
attempting to search Windows Update.
Exception Level 1:
 Error Message: Exception from HRESULT: 0x80240437
 Stack Trace: at WUApiLib.IUpdateSearcher.Search(String criteria)..
(Windows updates)
11/22/2020 09:17:30 UTC | Info | Searching for Windows Updates.
11/22/2020 09:18:59 UTC | Error | Searching for updates resulted in error: Exception from HRESULT: 0x80240437
----------ERROR-------
failed to run commands: exit status 4294967295
```

**Cause**: This output indicates that the native Windows Update APIs were unable to run the patching operations\.

**Solution**: Check the `HResult` code in the [Microsoft documentation](https://docs.microsoft.com/en-us/windows/deployment/update/windows-update-errors) to identify troubleshooting steps for resolving the error\.

## Troubleshooting instance does not have access to Windows Update Catalog or WSUS<a name="patch-manager-troubleshooting-instance-access"></a>

**Problem**: You received an error like the following\.

```
Downloading PatchBaselineOperations PowerShell module from https://s3.amazonaws.com/path_to_module.zip to C:\Windows\TEMP\Amazon.PatchBaselineOperations-1.29.zip.

Extracting PatchBaselineOperations zip file contents to temporary folder.

Verifying SHA 256 of the PatchBaselineOperations PowerShell module files.

Successfully downloaded and installed the PatchBaselineOperations PowerShell module.

Patch Summary for

PatchGroup :

BaselineId :

Baseline : null

SnapshotId :

RebootOption : RebootIfNeeded

OwnerInformation :

OperationType : Scan

OperationStartTime : 1970-01-01T00:00:00.0000000Z

OperationEndTime : 1970-01-01T00:00:00.0000000Z

InstalledCount : -1

InstalledRejectedCount : -1

InstalledPendingRebootCount : -1

InstalledOtherCount : -1

FailedCount : -1

MissingCount : -1

NotApplicableCount : -1

UnreportedNotApplicableCount : -1

EC2AMAZ-VL3099P - PatchBaselineOperations Assessment Results - 2020-12-30T20:59:46.169

----------ERROR-------

Invoke-PatchBaselineOperation : Exception Details: An error occurred when attempting to search Windows Update.

Exception Level 1:

Error Message: Exception from HRESULT: 0x80072EE2

Stack Trace: at WUApiLib.IUpdateSearcher.Search(String criteria)

at Amazon.Patch.Baseline.Operations.PatchNow.Implementations.WindowsUpdateAgent.SearchForUpdates(String

searchCriteria)

At C:\ProgramData\Amazon\SSM\InstanceData\i-02573cafcfEXAMPLE\document\orchestration\3d2d4864-04b7-4316-84fe-eafff1ea58

e3\PatchWindows\_script.ps1:230 char:13

+ $response = Invoke-PatchBaselineOperation -Operation Install -Snapsho ...

+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+ CategoryInfo : OperationStopped: (Amazon.Patch.Ba...UpdateOperation:InstallWindowsUpdateOperation) [Inv

oke-PatchBaselineOperation], Exception

+ FullyQualifiedErrorId : Exception Level 1:

Error Message: Exception Details: An error occurred when attempting to search Windows Update.

Exception Level 1:

Error Message: Exception from HRESULT: 0x80072EE2

Stack Trace: at WUApiLib.IUpdateSearcher.Search(String criteria)

at Amazon.Patch.Baseline.Operations.PatchNow.Implementations.WindowsUpdateAgent.SearchForUpdates(String searc

---Error truncated----
```

**Cause**: This error could be related to the Windows Update components, or to a lack of connectivity to the Windows Update Catalog or Windows Server Update Services \(WSUS\)\.

**Solution**: Confirm that the instance has connectivity to the [Microsoft Update Catalog](https://www.catalog.update.microsoft.com/home.aspx) through an internet gateway, NAT gateway, or NAT instance\. If you are using WSUS, confirm that the instance has connectivity to the WSUS server in your environment\. If connectivity is available to the intended destination, check the Microsoft documentation for other potential causes of `HResult 0x80072EE2`\. This might indicate an operating system level issue\. 

## Troubleshooting PatchBaseline module is not downloadable<a name="patch-manager-troubleshooting-module-not-downloadable"></a>

**Problem**: You received an error like the following\.

```
Preparing to download PatchBaselineOperations PowerShell module from S3.
Downloading PatchBaselineOperations PowerShell module from https://s3.amazonaws.com/path_to_module.zip to C:\Windows\TEMP\Amazon.PatchBaselineOperations-1.29.zip.
----------ERROR-------

C:\ProgramData\Amazon\SSM\InstanceData\i-02573cafcfEXAMPLE\document\orchestration\aaaaaaaa-bbbb-cccc-dddd-4f6ed6bd5514\

PatchWindows\_script.ps1 : An error occurred when executing PatchBaselineOperations: Unable to connect to the remote server

+ CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException

+ FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,_script.ps1

failed to run commands: exit status 4294967295
```

**Solution**: Check the instance connectivity and permissions to Amazon Simple Storage Service \(Amazon S3\)\. The instance's AWS Identity and Access Management \(IAM\) role must use the minimum permissions cited in [About minimum S3 Bucket permissions for SSM Agent](ssm-agent-minimum-s3-permissions.md)\. The instance must communicate with the Amazon S3 endpoint via Amazon S3 gateway endpoint, NAT gateway, or internet gateway\. For more information about the VPC Endpoint requirements for AWS Systems Manager SSM Agent \(SSM Agent\), see [Step 6: \(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md)\. 

## Troubleshooting missing patches<a name="patch-manager-troubleshooting-module-not-downloadable"></a>

**Problem**: `AWS-RunPatchbaseline` completed successfully, but there are some missing patches\.

Below are some common causes and their solutions\.

**Cause 1**: The baseline is not effective\.

**Solution 1**: To check if this is the cause, use the following procedure\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Select the **Command history** tab and then select the command whose baseline you want to check\.

1. Select the instance that has missing patches\.

1. Select **Step 1 \- Output** and find the `BaselineId` value\.

1. Check the assigned [patch baseline configuration](sysman-patch-baselines.md#patch-manager-baselines-custom), that is, the operating system, product name, classification, and severity for the patch baseline\.

1. Go to the [Microsoft Update Catalog](https://www.catalog.update.microsoft.com/home.aspx)\.

1. Search the Microsoft Knowledge Base \(KB\) article IDs \(for example, KB3216916\)\.

1. Verify that the value under **Product** matches that of your instance and select the corresponding **Title**\. A new **Update Details** window will open\.

1. In the **Overview** tab, the **classification** and **MSRC severity** must match the patch baseline configuration you found earlier\.

**Cause 2**: The patch was replaced\.

**Solution 2**: To check if this is true, use the following procedure\.

1. Go to the [Microsoft Update Catalog](https://www.catalog.update.microsoft.com/home.aspx)\.

1. Search the Microsoft Knowledge Base \(KB\) article IDs \(for example, KB3216916\)\.

1. Verify that the value under **Product** matches that of your instance and select the corresponding **Title**\. A new **Update Details** window will open\.

1. Go to the **Package Details** tab\. Look for an entry under the **This update has been replaced by the following updates:** header\.

**Cause 3**: The same patch might have different KB numbers because the WSUS and Window online updates are handled as independent Release Channels by Microsoft\.

**Solution 3**: Check the patch eligibility\. If the package is not available under WSUS, install [OS Build 14393\.3115](https://support.microsoft.com/en-us/topic/july-16-2019-kb4507459-os-build-14393-3115-511a3df6-c07e-14e3-dc95-b9898a7a7a57)\. If the package is available for all operating system builds, install [OS Builds 18362\.1256 and 18363\.1256](https://support.microsoft.com/en-us/topic/december-8-2020-kb4592449-os-builds-18362-1256-and-18363-1256-c448f3df-a5f1-1d55-aa31-0e1cf7a440a9)\.

## Contacting AWS Support<a name="patch-manager-troubleshooting-contact-support"></a>

If you can't find troubleshooting solutions in this section or in the [Systems Manager Developer Forum](https://forums.aws.amazon.com/forum.jspa?forumID=185) and if you have an AWS Support plan with Enhanced Technical Support, you can create a technical support case at [AWS Support](https://aws.amazon.com/premiumsupport/)\.

Before you contact AWS Support, collect the following items:
+ [SSM agent logs](sysman-agent-logs.md)
+ `%PROGRAMDATA%\Amazon\PatchBaselineOperations\Logs` as described on the Windows tab of [How patches are installed](patch-manager-how-it-works-installation.md)
+  Windows update logs: 

  For Windows 2012 R2 and older, use `%windir%/WindowsUpdate.log`

  For Windows 2016 and newer, first run the PowerShell command [https://docs.microsoft.com/en-us/powershell/module/windowsupdate/get-windowsupdatelog?view=win10-ps](https://docs.microsoft.com/en-us/powershell/module/windowsupdate/get-windowsupdatelog?view=win10-ps) before using `%windir%/WindowsUpdate.log`
+ Run Command command ID, maintenance window ID, or Automation execution ID