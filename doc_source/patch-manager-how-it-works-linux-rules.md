# How patch baseline rules work on Linux\-based systems<a name="patch-manager-how-it-works-linux-rules"></a>

The rules in a patch baseline for Linux distributions operate differently based on the distribution type\. Unlike patch updates on Windows Server instances, rules are evaluated on each instance to take the configured repos on the instance into consideration\. Patch Manager uses the native package manager to drive the installation of patches approved by the patch baseline\.

**Topics**
+ [How patch baseline rules work on Amazon Linux and Amazon Linux 2](#patch-manager-how-it-works-linux-rules-amazon-linux)
+ [How patch baseline rules work on CentOS](#patch-manager-how-it-works-linux-rules-centos)
+ [How patch baseline rules work on Debian](#patch-manager-how-it-works-linux-rules-debian)
+ [How patch baseline rules work on Oracle Linux](#patch-manager-how-it-works-linux-rules-oracle)
+ [How patch baseline rules work on RHEL](#patch-manager-how-it-works-linux-rules-rhel)
+ [How patch baseline rules work on SUSE Linux Enterprise Server](#patch-manager-how-it-works-linux-rules-sles)
+ [How patch baseline rules work on Ubuntu Server](#patch-manager-how-it-works-linux-rules-ubuntu)

## How patch baseline rules work on Amazon Linux and Amazon Linux 2<a name="patch-manager-how-it-works-linux-rules-amazon-linux"></a>

On Amazon Linux and Amazon Linux 2, the patch selection process is as follows:

1. On the instance, the YUM library accesses the `updateinfo.xml` file for each configured repo\. 
**Note**  
If no `updateinfo.xml` file is found, no patch will be applied\.

1. Each update notice in `updateinfo.xml` includes several attributes that denote the properties of the packages in the notice, as described in the following table\.  
**Update notice attributes**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-how-it-works-linux-rules.html)
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.

1. The product of the instance is determined by SSM Agent\. This attribute corresponds to the value of the Product key attribute in the patch baseline's [PatchFilter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PatchFilter.html) data type\.

1. Packages are selected for the update according to the following guidelines:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-how-it-works-linux-rules.html)

For information about patch compliance status values, see [About patch compliance status values](about-patch-compliance-states.md)\.

## How patch baseline rules work on CentOS<a name="patch-manager-how-it-works-linux-rules-centos"></a>

On CentOS, the patch selection process is as follows:

1. On the instance, the YUM library \(on CentOS 6\.x and 7\.x versions\) or the DNF library \(on CentOS 8\.x\) accesses the `updateinfo.xml` file for each configured repo\.
**Note**  
If there is no `updateinfo.xml` found, no patch will be applied\.

1. Each update notice in `updateinfo.xml` includes several attributes that denote the properties of the packages in the notice, as described in the following table\.  
**Update notice attributes**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-how-it-works-linux-rules.html)
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.

1. The product of the instance is determined by SSM Agent\. This attribute corresponds to the value of the Product key attribute in the patch baseline's [PatchFilter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PatchFilter.html) data type\.

1. Packages are selected for the update according to the following guidelines:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-how-it-works-linux-rules.html)

For information about patch compliance status values, see [About patch compliance status values](about-patch-compliance-states.md)\.

## How patch baseline rules work on Debian<a name="patch-manager-how-it-works-linux-rules-debian"></a>

On Debian, the patch baseline service offers filtering on the *Priority* and *Section *fields\. These fields are typically present for all Debian packages\. To determine whether a patch is selected by the patch baseline, Patch Manager does the following:

1. On Debian systems, the equivalent of `sudo apt-get update` is run to refresh the list of available packages\. Repos are not configured and the data is pulled from repos configured in a `sources` list\.
**Important**  
On Debian 8 only: Because Debian 8\.\* operating systems refer to an obsolete package repository \(`jessie-backports`\), Patch Manager performs the following additional steps to ensure that patching operations succeed:  
On your instance, the reference to the `jessie-backports` repository is commented out from the source location list \(`/etc/apt/sources.list.d/jessie-backports`\)\. As a result, no attempt is made to download patches from that location\.
A Stretch security update signing key is imported\. This key provides the necessary permissions for the update and install operations on Debian 8\.\* distributions\.
An `apt-get` operation is run to ensure that the latest version of `python3-apt` is installed before the patching process begins\.
After the installation process completes, the reference to the `jessie-backports` repository is restored and the signing key is removed from the apt sources keyring\. This is done to leave the system configuration as it was before the patching operation\. 

1. Next, the [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters), [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules), [ApprovedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) and [RejectedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) lists are applied\.
**Note**  
Because it's not possible to reliably determine the release dates of update packages for Debian, the auto\-approval options are not supported for this operating system\.

   Approval rules, however, are also subject to whether the **Include nonsecurity updates** check box was selected when creating or last updating a patch baseline\.

   If nonsecurity updates are excluded, an implicit rule is applied in order to select only packages with upgrades in security repos\. For each package, the candidate version of the package \(which is typically the latest version\) must be part of a security repo\. In this case, for Debian, patch candidate versions are limited to patches included in the following repos:

   These repos are named as follows:
   + Debian 8: `debian-security jessie`
   + Debian 9: `debian-security stretch`

   If nonsecurity updates are included, patches from other repositories are considered as well\.
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.

To view the contents of the *Priority* and *Section *fields, run the following `aptitude` command: 

**Note**  
You may need to first install Aptitude on Debian systems\.

```
aptitude search -F '%p %P %s %t %V#' '~U'
```

In the response to this command, all upgradable packages are reported in this format: 

```
name, priority, section, archive, candidate version
```

For information about patch compliance status values, see [About patch compliance status values](about-patch-compliance-states.md)\.

## How patch baseline rules work on Oracle Linux<a name="patch-manager-how-it-works-linux-rules-oracle"></a>

On Oracle Linux, the patch selection process is as follows:

1. On the instance, the YUM library accesses the `updateinfo.xml` file for each configured repo\.
**Note**  
The `updateinfo.xml` file might not be available if the repo is not one managed by Oracle\. If there is no `updateinfo.xml` found, no patch will be applied\.

1. Each update notice in `updateinfo.xml` includes several attributes that denote the properties of the packages in the notice, as described in the following table\.  
**Update notice attributes**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-how-it-works-linux-rules.html)
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.

1. The product of the instance is determined by SSM Agent\. This attribute corresponds to the value of the Product key attribute in the patch baseline's [PatchFilter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PatchFilter.html) data type\.

1. Packages are selected for the update according to the following guidelines:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-how-it-works-linux-rules.html)

For information about patch compliance status values, see [About patch compliance status values](about-patch-compliance-states.md)\.

## How patch baseline rules work on RHEL<a name="patch-manager-how-it-works-linux-rules-rhel"></a>

On Red Hat Enterprise Linux, the patch selection process is as follows:

1. On the instance, the YUM library \(RHEL 7\) or the DNF library \(RHEL 8\) accesses the `updateinfo.xml` file for each configured repo\.
**Note**  
The `updateinfo.xml` file might not be available if the repo is not one managed by Red Hat\. If there is no `updateinfo.xml` found, no patch will be applied\.

1. Each update notice in `updateinfo.xml` includes several attributes that denote the properties of the packages in the notice, as described in the following table\.  
**Update notice attributes**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-how-it-works-linux-rules.html)
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.

1. The product of the instance is determined by SSM Agent\. This attribute corresponds to the value of the Product key attribute in the patch baseline's [PatchFilter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PatchFilter.html) data type\.

1. Packages are selected for the update according to the following guidelines:    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-how-it-works-linux-rules.html)

For information about patch compliance status values, see [About patch compliance status values](about-patch-compliance-states.md)\.

## How patch baseline rules work on SUSE Linux Enterprise Server<a name="patch-manager-how-it-works-linux-rules-sles"></a>

On SLES, each patch includes the following attributes that denote the properties of the packages in the patch:
+ **Category**: Corresponds to the value of the **Classification** key attribute in the patch baseline's [PatchFilter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PatchFilter.html) data type\. Denotes the type of patch included in the update notice\.

  You can view the list of supported values by using the AWS CLI command [describe\-patch\-properties](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-patch-properties.html) or the API action [DescribePatchProperties](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribePatchProperties.html)\. You can also view the list in the **Approval rules** area of the **Create patch baseline** page or **Edit patch baseline** page in the Systems Manager console\.
+ **Severity**: Corresponds to the value of the **Severity** key attribute patch baseline's [PatchFilter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PatchFilter.html) data type\. Denotes the severity of the patches\.

  You can view the list of supported values by using the AWS CLI command [describe\-patch\-properties](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-patch-properties.html) or the API action [DescribePatchProperties](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribePatchProperties.html)\. You can also view the list in the **Approval rules** area of the **Create patch baseline** page or **Edit patch baseline** page in the Systems Manager console\.

The product of the instance is determined by SSM Agent\. This attribute corresponds to the value of the **Product** key attribute in the patch baseline's [PatchFilter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PatchFilter.html) data type\. 

For each patch, the patch baseline is used as a filter, allowing only the qualified packages to be included in the update\. If multiple packages are applicable after applying the patch baseline definition, the latest version is used\. 

**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.

## How patch baseline rules work on Ubuntu Server<a name="patch-manager-how-it-works-linux-rules-ubuntu"></a>

On Ubuntu Server, the patch baseline service offers filtering on the *Priority* and *Section *fields\. These fields are typically present for all Ubuntu Server packages\. To determine whether a patch is selected by the patch baseline, Patch Manager does the following:

1. On Ubuntu Server systems, the equivalent of `sudo apt-get update` is run to refresh the list of available packages\. Repos are not configured and the data is pulled from repos configured in a `sources` list\.

1. Next, the [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters), [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules), [ApprovedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) and [RejectedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) lists are applied\.
**Note**  
Because it's not possible to reliably determine the release dates of update packages for Ubuntu Server, the auto\-approval options are not supported for this operating system\.

   Approval rules, however, are also subject to whether the **Include nonsecurity updates** check box was selected when creating or last updating a patch baseline\.

   If nonsecurity updates are excluded, an implicit rule is applied in order to select only packages with upgrades in security repos\. For each package, the candidate version of the package \(which is typically the latest version\) must be part of a security repo\. In this case, for Ubuntu Server, patch candidate versions are limited to patches included in the following repos:
   + Ubuntu Server 14: `trusty-security`
   + Ubuntu Server 16: `xenial-security`
   + Ubuntu Server 18: `bionic-security`
   + Ubuntu Server 20: `focal-security`

   If nonsecurity updates are included, patches from other repositories are considered as well\.
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.

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

For information about patch compliance status values, see [About patch compliance status values](about-patch-compliance-states.md)\.