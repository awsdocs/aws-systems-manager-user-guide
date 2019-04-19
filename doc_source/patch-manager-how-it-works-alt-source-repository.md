# How to Specify an Alternative Patch Source Repository \(Linux\)<a name="patch-manager-how-it-works-alt-source-repository"></a>

When you use the default repositories configured on an instance for patching operations, Patch Manager scans for or installs security\-related patches\. This is the default behavior for Patch Manager\. For complete information on how Patch Manager selects and installs security patches, see [How Security Patches Are Selected](patch-manager-how-it-works-selection.md)\.

On Linux systems, however, you can also use Patch Manager to install patches that are not related to security, or that are in a different source repository than the default one configured on the instance\. You can specify alternative patch source repositories when you create a custom patch baseline\. In each custom patch baseline, you can specify patch source configurations for up to 20 versions of a supported Linux operating system\. 

For example, suppose that your Ubuntu Server fleet includes both Ubuntu Server 14\.04 and Ubuntu Server 16\.04 instances\. In this case, you can specify alternate repositories for each version in the same custom patch baseline\. For each version, you provide a name, specify the operating system version type \(product\), and provide a repository configuration\. You can also specify a single alternative source repository that applies to all versions of a supported operating system\.

For a list of example scenarios for using this option, see [Sample Uses for Alternative Patch Source Repositories](#patch-manager-how-it-works-alt-source-repository-examples) later in this topic\.

For information about default and custom patch baselines, see [Default and Custom Patch Baselines](sysman-patch-baselines.md)\.

**Note**  
Running a custom patch baseline that specifies alternative patch repositories on an instance doesn't change the default repository configured for the instance\.

**Using the Console**  
To specify alternative patch source repositories when you are working in the AWS Systems Manager console, use the **Patch sources** section on the **Create patch baseline** page\. For information about using the **Patch sources** options, see [Create a Default Patch Baseline](sysman-patch-baseline-console.md)\.

**Using Other Tools to Create Patch Baselines**  
Use the `sources` option with other tools when you create a patch baseline\. 
+ AWS CLI: [create\-patch\-baseline](https://docs.aws.amazon.com/cli/latest/reference/ssm/create-patch-baseline.html)
+ Systems Manager API: [API\_CreatePatchBaseline](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html)
+ Systems Manager AWS Tools for Windows PowerShell: [New\-SSMPatchBaseline](https://docs.aws.amazon.com/powershell/latest/reference/items/New-SSMPatchBaseline.html)

For an example of using the `--sources` option with the CLI, see [Create a patch baseline with custom repositories for different OS versions](patch-manager-cli-commands.md#patch-manager-cli-commands-create-patch-baseline-mult-sources)\.

**Topics**
+ [Important Considerations for Alternative Repositories](#alt-source-repository-important)
+ [Sample Uses for Alternative Patch Source Repositories](#patch-manager-how-it-works-alt-source-repository-examples)

## Important Considerations for Alternative Repositories<a name="alt-source-repository-important"></a>

Keep in mind the following points as you plan your patching strategy using alternative patch repositories\.

**Only specified repositories are used for patching**  
Specifying alternative repositories doesn't mean specifying *additional* repositories\. You can choose to specify repositories other than those configured as defaults on an instance\. However, you must also specify the default repositories as part of the alternative patch source configuration if you want their updates to be applied\.

For example, on Amazon Linux 2 instances, the default repositories are `amzn-main` and `amzn-update`\. If you want want to include the Extra Packages for Enterprise Linux \(EPEL\) repository in your patching operations, you must specify all three repositories as alternative repositories\.

**Note**  
Running a custom patch baseline that specifies alternative patch repositories on an instance doesn't change the default repository configured for the instance\.

**Patching behavior for YUM\-based distributions depends on the updateinfo\.xml manifest**  
When you specify alternative patch repositories for YUM\-based distributions, such as Amazon Linux or Amazon Linux 2, Red Hat Enterprise Linux, or CentOS, patching behavior depends on whether the repository includes an update manifest in the form of a complete and correctly formatted `updateinfo.xml` file\. This file specifies the release date, classifications, and severities of the various packages\. Any of the following will affect the patching behavior:
+ If you filter on **Classification** and **Severity**, but they aren't specified in `updateinfo.xml`, the package will not be included by the filter\. This also means that packages without an `updateinfo.xml` file won't be included in patching\.
+ If you filter on **ApprovalAfterDays**, but the package release date isn't in Unix Epoch format \(or has no release date specified\), the package will not be included by the filter\.
+ There is an exception if you select the **Approved patches include non\-security updates** check box in the **Create patch baseline** page\. In this case, packages without an `updateinfo.xml` file \(or that contains this file without properly formatted **Classification**, **Severity**, and **Date** values\) *will* be included in the prefiltered list of patches\. \(They must still meet the other patch baseline rule requirements in order to be installed\.\)

## Sample Uses for Alternative Patch Source Repositories<a name="patch-manager-how-it-works-alt-source-repository-examples"></a>

**Example 1 – Nonsecurity Updates for Ubuntu Server**  
You are already using Patch Manager to install security patches on a fleet of Ubuntu Server instances using the AWS\-provided default patch baseline **AWS\-UbuntuDefaultPatchBaseline**\. You can create a new patch baseline that is based on this default, but specify in the approval rules that you want nonsecurity related updates that are part of the default distribution to be installed as well\. When this patch baseline is run against your instances, patches for both security and nonsecurity issues are applied\. You can also choose to approve nonsecurity patches in the patch exceptions you specify for a baseline\.

**Example 2 \- Personal Package Archives \(PPA\) for Ubuntu Server**  
Your Ubuntu Server instances are running software that is distributed through a [Personal Package Archives \(PPA\) for Ubuntu](https://launchpad.net/ubuntu/+ppas)\. In this case, you create a patch baseline that specifies a PPA repository that you have configured on the instance as the source repository for the patching operation\. Then use Run Command to run the patch baseline document on the instances\.

**Example 3 – Internal Corporate Applications on Amazon Linux**  
You need to run some applications needed for industry regulatory compliance on your Amazon Linux instances\. You can configure a repository for these applications on the instances, use YUM to initially install the applications, and then update or create a new patch baseline to include this new corporate repository\. After this you can use Run Command to run the **AWS\-RunPatchBaseline** document with the Scan option to see if the corporate package is listed among the installed packages and is up to date on the instance\. If it isn't up to date, you can run the document again using the Install option to update the applications\. 