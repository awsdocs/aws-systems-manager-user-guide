# Troubleshooting Patch Manager<a name="patch-manager-troubleshooting"></a>

Use the following information to help you troubleshoot problems with Patch Manager, a capability of AWS Systems Manager\.

**Topics**
+ [Errors when running `AWS-RunPatchBaseline` on Linux](#patch-manager-troubleshooting-linux)
+ [Errors when running `AWS-RunPatchBaseline` on Windows Server](#patch-manager-troubleshooting-windows)
+ [Contacting AWS Support](#patch-manager-troubleshooting-contact-support)

## Errors when running `AWS-RunPatchBaseline` on Linux<a name="patch-manager-troubleshooting-linux"></a>

**Topics**
+ [Issue: 'No such file or directory' error](#patch-manager-troubleshooting-linux-1)
+ [Issue: 'another process has acquired yum lock' error](#patch-manager-troubleshooting-linux-2)
+ [Issue: 'Permission denied / failed to run commands' error](#patch-manager-troubleshooting-linux-3)
+ [Issue: 'Unable to download payload' error](#patch-manager-troubleshooting-linux-4)
+ [Issue: 'unsupported package manager and python version combination' error](#patch-manager-troubleshooting-linux-5)
+ [Issue: Patch Manager isn't applying rules specified to exclude certain packages](#patch-manager-troubleshooting-linux-6)
+ [Issue: Patching fails and Patch Manager reports that the Server Name Indication extension to TLS is not available](#patch-manager-troubleshooting-linux-7)
+ [Issue: Patch Manager reports 'No more mirrors to try'](#patch-manager-troubleshooting-linux-8)

### Issue: 'No such file or directory' error<a name="patch-manager-troubleshooting-linux-1"></a>

**Problem**: When you run `AWS-RunPatchBaseline`, patching fails with one of the following errors\.

```
IOError: [Errno 2] No such file or directory: 'patch-baseline-operations-X.XX.tar.gz'
```

```
Unable to extract tar file: /var/log/amazon/ssm/patch-baseline-operations/patch-baseline-operations-1.75.tar.gz.failed to run commands: exit status 155
```

```
Unable to load and extract the content of payload, abort.failed to run commands: exit status 152
```

**Cause 1**: Two commands to run `AWS-RunPatchBaseline` were running at the same time on the same managed node\. This creates a race condition that results in the temporary `file patch-baseline-operations*` not being created or accessed properly\.

**Cause 2**: Insufficient storage space remains under the `/var` directory\. 

**Solution 1**: Ensure that no maintenance window has two or more Run Command tasks that run `AWS-RunPatchBaseline` with the same Priority level and that run on the same target IDs\. If this is the case, reorder the priority\. Run Command is a capability of AWS Systems Manager\.

**Solution 2**: Ensure that only one maintenance window at a time is running Run Command tasks that use `AWS-RunPatchBaseline` on the same targets and on the same schedule\. If this is the case, change the schedule\.

**Solution 3**: Ensure that only one State Manager association is running `AWS-RunPatchBaseline` on the same schedule and targeting the same managed nodes\. State Manager is a capability of AWS Systems Manager\.

**Solution 4**: Free up sufficient storage space under the `/var` directory for the update packages\.

### Issue: 'another process has acquired yum lock' error<a name="patch-manager-troubleshooting-linux-2"></a>

**Problem**: When you run `AWS-RunPatchBaseline`, patching fails with the following error\.

```
12/20/2019 21:41:48 root [INFO]: another process has acquired yum lock, waiting 2 s and retry.
```

**Cause**: The `AWS-RunPatchBaseline` document has started running on a managed node where it's already running in another operation and and has acquired the package manager `yum` process\.

**Solution**: Ensure that no State Manager association, maintenance window tasks, or other configurations that run `AWS-RunPatchBaseline` on a schedule\) are targeting the same managed node around the same time\.

### Issue: 'Permission denied / failed to run commands' error<a name="patch-manager-troubleshooting-linux-3"></a>

**Problem**: When you run `AWS-RunPatchBaseline`, patching fails with the following error\.

```
sh: 
/var/lib/amazon/ssm/instanceid/document/orchestration/commandid/PatchLinux/_script.sh: Permission denied
failed to run commands: exit status 126
```

**Cause**: `/var/lib/amazon/` might be mounted with `noexec` permissions\. This is an issue because SSM Agent downloads payload scripts to `/var/lib/amazon/ssm` and runs them from that location\.

**Solution**: Ensure that you have have configured exclusive partitions to `/var/log/amazon` and `/var/lib/amazon`, and that they're mounted with `exec` permissions\.

### Issue: 'Unable to download payload' error<a name="patch-manager-troubleshooting-linux-4"></a>

**Problem**: When you run `AWS-RunPatchBaseline`, patching fails with the following error\.

```
Unable to download payload: https://s3.DOC-EXAMPLE-BUCKET.region.amazonaws.com/aws-ssm-region/patchbaselineoperations/linux/payloads/patch-baseline-operations-X.XX.tar.gz.failed to run commands: exit status 156
```

**Cause**: The managed node doesn't have the required permissions to access the specified Amazon Simple Storage Service \(Amazon S3\) bucket\.

**Solution**: Update your network configuration so that S3 endpoints are reachable\. For more details, see information about required access to S3 buckets for Patch Manager in [SSM Agent communications with AWS managed S3 buckets](ssm-agent-minimum-s3-permissions.md)\.

### Issue: 'unsupported package manager and python version combination' error<a name="patch-manager-troubleshooting-linux-5"></a>

**Problem**: When you run `AWS-RunPatchBaseline`, patching fails with the following error\.

```
An unsupported package manager and python version combination was found. Apt requires Python3 to be installed.
failed to run commands: exit status 1
```

**Cause**: python3 isn't installed on the Debian Server, Raspberry Pi OS, or Ubuntu Server instance\.

**Solution**: Install python3 on the server, which is required for Debian Server, Raspberry Pi OS, and Ubuntu Server managed nodes\.

### Issue: Patch Manager isn't applying rules specified to exclude certain packages<a name="patch-manager-troubleshooting-linux-6"></a>

**Problem**: You have attempted to exclude certain packages by specifying them in the `/etc/yum.conf` file, in the format `exclude=package-name`, but they aren't excluded during the Patch Manager `Install` operation\.

**Cause**: Patch Manager doesn't incorporate exclusions specified in the `/etc/yum.conf` file\.

**Solution**: To exclude specific packages, create a custom patch baseline and create a rule to exclude the packages you don't want installed\.

### Issue: Patching fails and Patch Manager reports that the Server Name Indication extension to TLS is not available<a name="patch-manager-troubleshooting-linux-7"></a>

**Problem**: The patching operation issues the following message\.

```
/var/log/amazon/ssm/patch-baseline-operations/urllib3/util/ssl_.py:369: 
SNIMissingWarning: An HTTPS request has been made, but the SNI (Server Name Indication) extension
to TLS is not available on this platform. This may cause the server to present an incorrect TLS 
certificate, which can cause validation failures. You can upgrade to a newer version of Python 
to solve this. 
For more information, see https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings
```

**Cause**: This message doesn't indicate an error\. Instead, it's a warning that the older version of Python distributed with the operating system doesn't support TLS Server Name Indication\. The Systems Manager patch payload script issues this warning when connecting to AWS APIs that support SNI\.

**Solution**: To troubleshoot any patching failures when this message is reported, review the contents of the `stdout` and `stderr` files\. If you haven't configured the patch baseline to store these files in an Amazon S3 bucket or in Amazon CloudWatch Logs, you can locate the files in the following location on your Linux managed node\. 

`/var/lib/amazon/ssm/instance-id/document/orchestration/Run-Command-execution-id/awsrunShellScript/PatchLinux`

### Issue: Patch Manager reports 'No more mirrors to try'<a name="patch-manager-troubleshooting-linux-8"></a>

**Problem**: The patching operation issues the following message\.

```
[Errno 256] No more mirrors to try.
```

**Cause**: The repositories configured on the managed node are not working correctly\. Possible causes for this include:
+ The `yum` cache is corrupted\.
+ A repository URL can't be reached due to network\-related issues\.

**Solution**: Patch Manager uses the managed node’s default package manager to perform patching operation\. Double\-check that repositories are configured and operating correctly\.

## Errors when running `AWS-RunPatchBaseline` on Windows Server<a name="patch-manager-troubleshooting-windows"></a>

**Topics**
+ [Issue: mismatched product family/product pairs](#patch-manager-troubleshooting-product-family-mismatch)
+ [Issue: `AWS-RunPatchBaseline` output returns an `HRESULT` \(Windows Server\)](#patch-manager-troubleshooting-hresult)
+ [Issue: managed node doesn't have access to Windows Update Catalog or WSUS](#patch-manager-troubleshooting-instance-access)
+ [Issue: PatchBaselineOperations PowerShell module is not downloadable](#patch-manager-troubleshooting-module-not-downloadable)
+ [Issue: missing patches](#patch-manager-troubleshooting-missing-patches)

### Issue: mismatched product family/product pairs<a name="patch-manager-troubleshooting-product-family-mismatch"></a>

**Problem**: When you create a patch baseline in the Systems Manager console, you specify a product family and a product\. For example, you might choose:
+ **Product family**: **Office**

  **Product**: **Office 2016**

**Cause**: If you attempt to create a patch baseline with a mismatched product family/product pair, an error message is displayed\. The following are reasons this can occur:
+ You selected a valid product family and product pair but then removed the product family selection\.
+ You chose a product from the **Obsolete or mismatched options** sublist instead of the **Available and matching options** sublist\. 

  Items in the product **Obsolete or mismatched options** sublist might have been entered in error through an SDK or AWS Command Line Interface \(AWS CLI\) `create-patch-baseline` command\. This could mean a typo was introduced or a product was assigned to the wrong product family\. A product is also included in the **Obsolete or mismatched options** sublist if it was specified for a previous patch baseline but has no patches available from Microsoft\. 

**Solution**: To avoid this issue in the console, always choose options from the **Currently available options** sublists\.

You can also view the products that have available patches by using the `[describe\-patch\-properties](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-patch-properties.html)` command in the AWS CLI or the `[DescribePatchProperties](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribePatchProperties.html)` API command\.

### Issue: `AWS-RunPatchBaseline` output returns an `HRESULT` \(Windows Server\)<a name="patch-manager-troubleshooting-hresult"></a>

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

### Issue: managed node doesn't have access to Windows Update Catalog or WSUS<a name="patch-manager-troubleshooting-instance-access"></a>

**Problem**: You received an error like the following\.

```
Downloading PatchBaselineOperations PowerShell module from https://s3.aws-api-domain/path_to_module.zip to C:\Windows\TEMP\Amazon.PatchBaselineOperations-1.29.zip.

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

**Solution**: Confirm that the managed node has connectivity to the [Microsoft Update Catalog](https://www.catalog.update.microsoft.com/home.aspx) through an internet gateway, NAT gateway, or NAT instance\. If you're using WSUS, confirm that the managed node has connectivity to the WSUS server in your environment\. If connectivity is available to the intended destination, check the Microsoft documentation for other potential causes of `HResult 0x80072EE2`\. This might indicate an operating system level issue\. 

### Issue: PatchBaselineOperations PowerShell module is not downloadable<a name="patch-manager-troubleshooting-module-not-downloadable"></a>

**Problem**: You received an error like the following\.

```
Preparing to download PatchBaselineOperations PowerShell module from S3.
                    
Downloading PatchBaselineOperations PowerShell module from https://s3.aws-api-domain/path_to_module.zip to C:\Windows\TEMP\Amazon.PatchBaselineOperations-1.29.zip.
----------ERROR-------

C:\ProgramData\Amazon\SSM\InstanceData\i-02573cafcfEXAMPLE\document\orchestration\aaaaaaaa-bbbb-cccc-dddd-4f6ed6bd5514\

PatchWindows\_script.ps1 : An error occurred when executing PatchBaselineOperations: Unable to connect to the remote server

+ CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException

+ FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,_script.ps1

failed to run commands: exit status 4294967295
```

**Solution**: Check the managed node connectivity and permissions to Amazon Simple Storage Service \(Amazon S3\)\. The managed node's AWS Identity and Access Management \(IAM\) role must use the minimum permissions cited in [SSM Agent communications with AWS managed S3 buckets](ssm-agent-minimum-s3-permissions.md)\. The node must communicate with the Amazon S3 endpoint through the Amazon S3 gateway endpoint, NAT gateway, or internet gateway\. For more information about the VPC Endpoint requirements for AWS Systems Manager SSM Agent \(SSM Agent\), see [Step 6: \(Recommended\) Create a VPC endpoint](setup-create-vpc.md)\. 

### Issue: missing patches<a name="patch-manager-troubleshooting-missing-patches"></a>

**Problem**: `AWS-RunPatchbaseline` completed successfully, but there are some missing patches\.

The following are some common causes and their solutions\.

**Cause 1**: The baseline isn't effective\.

**Solution 1**: To check if this is the cause, use the following procedure\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Select the **Command history** tab and then select the command whose baseline you want to check\.

1. Select the managed node that has missing patches\.

1. Select **Step 1 \- Output** and find the `BaselineId` value\.

1. Check the assigned [patch baseline configuration](sysman-patch-baselines.md#patch-manager-baselines-custom), that is, the operating system, product name, classification, and severity for the patch baseline\.

1. Go to the [Microsoft Update Catalog](https://www.catalog.update.microsoft.com/home.aspx)\.

1. Search the Microsoft Knowledge Base \(KB\) article IDs \(for example, KB3216916\)\.

1. Verify that the value under **Product** matches that of your managed node and select the corresponding **Title**\. A new **Update Details** window will open\.

1. In the **Overview** tab, the **classification** and **MSRC severity** must match the patch baseline configuration you found earlier\.

**Cause 2**: The patch was replaced\.

**Solution 2**: To check if this is true, use the following procedure\.

1. Go to the [Microsoft Update Catalog](https://www.catalog.update.microsoft.com/home.aspx)\.

1. Search the Microsoft Knowledge Base \(KB\) article IDs \(for example, KB3216916\)\.

1. Verify that the value under **Product** matches that of your managed node and select the corresponding **Title**\. A new **Update Details** window will open\.

1. Go to the **Package Details** tab\. Look for an entry under the **This update has been replaced by the following updates:** header\.

**Cause 3**: The same patch might have different KB numbers because the WSUS and Window online updates are handled as independent Release Channels by Microsoft\.

**Solution 3**: Check the patch eligibility\. If the package isn't available under WSUS, install [OS Build 14393\.3115](https://support.microsoft.com/en-us/topic/july-16-2019-kb4507459-os-build-14393-3115-511a3df6-c07e-14e3-dc95-b9898a7a7a57)\. If the package is available for all operating system builds, install [OS Builds 18362\.1256 and 18363\.1256](https://support.microsoft.com/en-us/topic/december-8-2020-kb4592449-os-builds-18362-1256-and-18363-1256-c448f3df-a5f1-1d55-aa31-0e1cf7a440a9)\.

## Contacting AWS Support<a name="patch-manager-troubleshooting-contact-support"></a>

If you can't find troubleshooting solutions in this section or in the Systems Manager issues in [AWS re:Post](https://repost.aws/tags/TA-UbbRGVYRWCDaCvae6itYg/aws-systems-manager), and you have a [Developer, Business, or Enterprise AWS Support plan](http://aws.amazon.com/premiumsupport/plans), you can create a technical support case at [AWS Support](https://aws.amazon.com/premiumsupport/)\.

Before you contact AWS Support, collect the following items:
+ [SSM agent logs](sysman-agent-logs.md)
+ Run Command command ID, maintenance window ID, or Automation execution ID
+ For Windows Server managed nodes, also collect the following:
  + `%PROGRAMDATA%\Amazon\PatchBaselineOperations\Logs` as described on the **Windows** tab of [How patches are installed](patch-manager-how-it-works-installation.md)
  + Windows update logs: For Windows Server 2012 R2 and older, use `%windir%/WindowsUpdate.log`\. For Windows Server 2016 and newer, first run the PowerShell command [https://docs.microsoft.com/en-us/powershell/module/windowsupdate/get-windowsupdatelog?view=win10-ps](https://docs.microsoft.com/en-us/powershell/module/windowsupdate/get-windowsupdatelog?view=win10-ps) before using `%windir%/WindowsUpdate.log`
+ For Linux managed nodes, also collect the following:
  + The contents of the file `/var/lib/amazon/ssm/instance-id/document/orchestration/Run-Command-execution-id/awsrunShellScript/PatchLinux`