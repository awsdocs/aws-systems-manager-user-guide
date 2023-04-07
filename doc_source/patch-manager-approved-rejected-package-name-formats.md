# About package name formats for approved and rejected patch lists<a name="patch-manager-approved-rejected-package-name-formats"></a>

The formats of package names you can add to lists of approved patches and rejected patches depend on the type of operating system you're patching\.

## Package name formats for Linux operating systems<a name="patch-manager-approved-rejected-package-name-formats-linux"></a>

The formats you can specify for approved and rejected patches in your patch baseline vary by Linux type\. More specifically, the formats that are supported depend on the package manager used by the type of Linux operating system\.

**Topics**
+ [Amazon Linux, Amazon Linux 2, Amazon Linux 2022, Amazon Linux 2023, CentOS, Oracle Linux, and Red Hat Enterprise Linux \(RHEL\)](#patch-manager-approved-rejected-package-name-formats-standard)
+ [Debian Server, Raspberry Pi OS \(formerly Raspbian\), and Ubuntu Server](#patch-manager-approved-rejected-package-name-formats-ubuntu)
+ [SUSE Linux Enterprise Server \(SLES\)](#patch-manager-approved-rejected-package-name-formats-sles)

### Amazon Linux, Amazon Linux 2, Amazon Linux 2022, Amazon Linux 2023, CentOS, Oracle Linux, and Red Hat Enterprise Linux \(RHEL\)<a name="patch-manager-approved-rejected-package-name-formats-standard"></a>

**Package manager**: YUM, except for Amazon Linux 2022, Amazon Linux 2023, RHEL 8, and CentOS 8, which use DNF as the package manager

**Approved patches**: For approved patches, you can specify any of the following:
+ Bugzilla IDs, in the format `1234567` \(The system processes numbers\-only strings as Bugzilla IDs\.\)
+ CVE IDs, in the format `CVE-2018-1234567`
+ Advisory IDs, in formats such as `RHSA-2017:0864` and `ALAS-2018-123`
+ Full package names, in formats such as:
  + `example-pkg-0.710.10-2.7.abcd.x86_64` 
  + `pkg-example-EE-20180914-2.2.amzn1.noarch`
+ Package\-names with a single wildcard, in formats such as:
  + `example-pkg-*.abcd.x86_64` 
  + `example-pkg-*-20180914-2.2.amzn1.noarch`
  + `example-pkg-EE-2018*.amzn1.noarch`

**Rejected patches**: For rejected patches, you can specify any of the following:
+ Full package names, in formats such as:
  + `example-pkg-0.710.10-2.7.abcd.x86_64` 
  + `pkg-example-EE-20180914-2.2.amzn1.noarch`
+ Package\-names with a single wildcard, in formats such as:
  + `example-pkg-*.abcd.x86_64` 
  + `example-pkg-*-20180914-2.2.amzn1.noarch`
  + `example-pkg-EE-2018*.amzn1.noarch`

### Debian Server, Raspberry Pi OS \(formerly Raspbian\), and Ubuntu Server<a name="patch-manager-approved-rejected-package-name-formats-ubuntu"></a>

**Package manager**: APT

**Approved patches** and **rejected patches**: For both approved and rejected patches, specify the following:
+ Package names, in the format `ExamplePkg33`
**Note**  
For Debian Server lists, Raspberry Pi OS lists, and Ubuntu Server lists, don't include elements such as architecture or versions\. For example, you specify the package name `ExamplePkg33` to include all the following in a patch list:  
`ExamplePkg33.x86.1`
`ExamplePkg33.x86.2`
`ExamplePkg33.x64.1`
`ExamplePkg33.3.2.5-364.noarch`

### SUSE Linux Enterprise Server \(SLES\)<a name="patch-manager-approved-rejected-package-name-formats-sles"></a>

**Package manager**: Zypper

**Approved patches** and **rejected patches**: For both approved and rejected patch lists, you can specify any of the following:
+ Full package names, in formats such as:
  + `SUSE-SLE-Example-Package-12-2018-123`
  + `example-pkg-2018.11.4-46.17.1.x86_64.rpm`
+ Package names with a single wildcard, such as:
  + `SUSE-SLE-Example-Package-12-2018-*`
  + `example-pkg-2018.11.4-46.17.1.*.rpm`

## Package name formats for macOS<a name="patch-manager-approved-rejected-package-name-formats-macos"></a>

**Supported package managers**: softwareupdate, installer, Brew, Brew Cask

**Approved patches** and **rejected patches**: For both approved and rejected patch lists, you specify full package names, in formats such as:
+ `XProtectPlistConfigData`
+ `MRTConfigData`

Wildcards aren't supported in approved and rejected patch lists for macOS\.

## Package name formats for Windows operating systems<a name="patch-manager-approved-rejected-package-name-formats-windows"></a>

For Windows operating systems, specify patches using Microsoft Knowledge Base IDs and Microsoft Security Bulletin IDs; for example:

```
KB2032276,KB2124261,MS10-048
```