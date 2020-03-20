# How Patch Baseline Rules Work on Linux\-Based Systems<a name="patch-manager-how-it-works-linux-rules"></a>

The rules in a patch baseline for Linux distributions operate differently based on the distribution type\. Unlike patch updates on Windows instances, rules are evaluated on each instance to take the configured repos on the instance into consideration\. Patch Manager uses the native package manager to drive the installation of patches approved by the patch baseline\.

**Topics**
+ [How Patch Baseline Rules Work on Amazon Linux and Amazon Linux 2](#patch-manager-how-it-works-linux-rules-amazon-linux)
+ [How Patch Baseline Rules Work on RHEL](#patch-manager-how-it-works-linux-rules-rhel)
+ [How Patch Baseline Rules Work on Ubuntu Server](#patch-manager-how-it-works-linux-rules-ubuntu)
+ [How Patch Baseline Rules Work on SUSE Linux Enterprise Server](#patch-manager-how-it-works-linux-rules-sles)

## How Patch Baseline Rules Work on Amazon Linux and Amazon Linux 2<a name="patch-manager-how-it-works-linux-rules-amazon-linux"></a>

On Amazon Linux and Amazon Linux 2, the patch selection process is as follows:

1. On the instance, the YUM library accesses the `updateinfo.xml` file for each configured repo\. 
**Note**  
If no `updateinfo.xml` file is found, no patch will be applied\.

1. Each update notice in `updateinfo.xml` includes several attributes that denote the properties of the packages in the notice, as described in the following table\.  
**Update Notice Attributes**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-how-it-works-linux-rules.html)
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About Package Name Formats for Approved and Rejected Patch Lists](patch-manager-approved-rejected-package-name-formats.md)\.

1. The product of the instance is determined by SSM Agent\. This attribute corresponds to the value of the Product key attribute in the patch baseline's [PatchFilter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PatchFilter.html) data type\.

1. Packages are selected for the update according to the follow guidelines:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-how-it-works-linux-rules.html)

For information about patch compliance status values, see [About Patch Compliance Status Values](about-patch-compliance-states.md)\.

## How Patch Baseline Rules Work on RHEL<a name="patch-manager-how-it-works-linux-rules-rhel"></a>

On Red Hat Enterprise Linux, the patch selection process is as follows:

1. On the instance, the YUM library accesses the `updateinfo.xml` file for each configured repo\.
**Note**  
The `updateinfo.xml` file might not be available if the repo is not one managed by Red Hat\. If there is no `updateinfo.xml` found, no patch will be applied\.

1. Each update notice in `updateinfo.xml` includes several attributes that denote the properties of the packages in the notice, as described in the following table\.  
**Update Notice Attributes**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-how-it-works-linux-rules.html)
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About Package Name Formats for Approved and Rejected Patch Lists](patch-manager-approved-rejected-package-name-formats.md)\.

1. The product of the instance is determined by SSM Agent\. This attribute corresponds to the value of the Product key attribute in the patch baseline's [PatchFilter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PatchFilter.html) data type\.

1. Packages are selected for the update according to the follow guidelines:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-how-it-works-linux-rules.html)

For information about patch compliance status values, see [About Patch Compliance Status Values](about-patch-compliance-states.md)\.

## How Patch Baseline Rules Work on Ubuntu Server<a name="patch-manager-how-it-works-linux-rules-ubuntu"></a>

On Ubuntu Server, the patch baseline service offers filtering on the *Priority* and *Section *fields\. These fields are typically present for all Ubuntu Server packages\. To determine whether a patch is selected by the patch baseline, Patch Manager does the following:

1. On Ubuntu systems, the equivalent of `sudo apt-get update` is run to refresh the list of available packages\. Repos are not configured and the data is pulled from repos configured in a `sources` list\.

1. Next, the [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-GlobalFilters), [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules), [ApprovedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) and [RejectedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) lists are applied\. Only packages with candidate versions appearing in the distribution security repo \(archive\) are selected\. For Ubuntu Server 14 this is repo is `trusty-security`\. For Ubuntu Server 16, it is `xenial-security`\.
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About Package Name Formats for Approved and Rejected Patch Lists](patch-manager-approved-rejected-package-name-formats.md)\.

To view the contents of the *Priority* and *Section *fields, run the following `aptitude` command: 

**Note**  
You may need to first install Aptitude on Ubuntu Server 16 systems\.

```
aptitude search -F '%p %P %s %t %V#' '~U'
```

In the response to this command, all upgradable packages are reported in this format: 

```
name, priority, section, archive, candidate version
```

For information about patch compliance status values, see [About Patch Compliance Status Values](about-patch-compliance-states.md)\.

## How Patch Baseline Rules Work on SUSE Linux Enterprise Server<a name="patch-manager-how-it-works-linux-rules-sles"></a>

On SLES, each patch includes the following attributes that denote the properties of the packages in the patch:
+ **Category**: Corresponds to the value of the **Classification** key attribute in the patch baseline's [PatchFilter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PatchFilter.html) data type\. Denotes the type of patch included in the update notice\.

  You can view the list of supported values by using the AWS CLI command [describe\-patch\-properties](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-patch-properties.html) or the API action [DescribePatchProperties](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribePatchProperties.html)\. You can also view the list in the **Approval rules** area of the **Create patch baseline** page or **Edit patch baseline** page in the Systems Manager console\.
+ **Severity**: Corresponds to the value of the **Severity** key attribute patch baseline's [PatchFilter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PatchFilter.html) data type\. Denotes the severity of the patches\.

  You can view the list of supported values by using the AWS CLI command [describe\-patch\-properties](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-patch-properties.html) or the API action [DescribePatchProperties](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribePatchProperties.html)\. You can also view the list in the **Approval rules** area of the **Create patch baseline** page or **Edit patch baseline** page in the Systems Manager console\.

The product of the instance is determined by SSM Agent\. This attribute corresponds to the value of the **Product** key attribute in the patch baseline's [PatchFilter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PatchFilter.html) data type\. 

For each patch, the patch baseline is used as a filter, allowing only the qualified packages to be included in the update\. If multiple packages are applicable after applying the patch baseline definition, the latest version is used\. 

**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About Package Name Formats for Approved and Rejected Patch Lists](patch-manager-approved-rejected-package-name-formats.md)\.