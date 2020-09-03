# How security patches are selected<a name="patch-manager-how-it-works-selection"></a>

The primary focus of Patch Manager is on installing operating systems security\-related updates on instances\. By default, Patch Manager doesn't install all available patches, but rather a smaller set of patches focused on security\.

**Note**  
On all Linux\-based systems supported by Patch Manager, you can choose a different source repository configured for the instance, typically to install nonsecurity updates\. For information, see [How to specify an alternative patch source repository \(Linux\)](patch-manager-how-it-works-alt-source-repository.md)\.

Choose from the following tabs to learn how Patch Manager selects security patches for your operating system\.

------
#### [ Amazon Linux and Amazon Linux 2 ]

On Amazon Linux and Amazon Linux 2, the Systems Manager patch baseline service uses preconfigured repositories on the instance\. There are usually two preconfigured repositories \(repos\) on an instance:
+ **Repo ID**: amzn\-main/latest

  **Repo name**: amzn\-main\-Base
+ **Repo ID**: amzn\-updates/latest

  **Repo name**: amzn\-updates\-Base

**Note**  
All updates are downloaded from the remote repos configured on the instance\. Therefore, the instance must be able to connect to the repos so the patching can be performed\.

Amazon Linux and Amazon Linux 2 instances use Yum as the package manager, and Yum uses the concept of an update notice as a file named `updateinfo.xml`\. An update notice is simply a collection of packages that fix specific problems\. All packages that are in an update notice are considered Security by Patch Manager\. Individual packages are not assigned classifications or severity levels\. For this reason, Patch Manager assigns the attributes of an update notice to the related packages\.

**Note**  
If you select the **Approved patches include non\-security updates** check box in the **Create patch baseline** page, then packages that are not classified in an `updateinfo.xml` file \(or a package that contains a file without properly formatted Classification, Severity, and Date values\) can be included in the prefiltered list of patches\. However, in order for a patch to be applied, the patch must still meet the user\-specified patch baseline rules\.

------
#### [ CentOS ]

On CentOS, the Systems Manager patch baseline service uses preconfigured repositories \(repos\) on the instance\. Here are some examples from a CentOS 6\.9 Amazon Machine Image \(AMI\):
+ **Repo ID**: ultra\-centos\-6\.9\-base

  **Repo name**: UltraServe CentOS\-6\.9 \- Base
+ **Repo ID**: ultra\-centos\-6\.9\-extras 

  **Repo name**: UltraServe CentOS\-6\.9 \- Extras
+ **Repo ID**: ultra\-centos\-6\.9\-updates

  **Repo name**: UltraServe CentOS\-6\.9 \- Updates
+ **Repo ID**: ultra\-centos\-6\.x\-glusterfs

  **Repo name**: UltraServe CentOS\-6\.x \- GlusterFS
+ **Repo ID**: ultra\-centos\-6\.x\-ultrarepo

  **Repo name**: UltraServe CentOS\-6\.x – UltraServe Repo Packages

**Note**  
All updates are downloaded from the remote repos configured on the instance\. Therefore, the instance must be able to connect to the repos so the patching can be performed\.

CentOS 6 and 7 instances use Yum as the package manager\. CentOS 8 instances use DNF as the package manager\. Both package managers use the concept of an update notice\. An update notice is simply a collection of packages that fix specific problems\. All packages that are in an update notice are considered Security packages by Patch Manager\.

However, CentOS default repos aren't configured with an update notice\. This means that Patch Manager does not detect packages on a default CentOS repo\. To enable Patch Manager to process packages that aren't contained in an update notice, you must enable the `EnableNonSecurity` flag in the patch baseline rules\.

**Note**  
CentOS update notices are supported\. Repos with update notices can be downloaded after launch\.

------
#### [ Debian ]

On Debian, the Systems Manager patch baseline service uses preconfigured repositories \(repos\) on the instance\. These preconfigured repos are used to pull an updated list of available package upgrades\. For this, Systems Manager performs the equivalent of a `sudo apt-get update` command\. 

Packages are then filtered from `debian-security codename` repos, where the codename is something like `jessie` or `stretch`\. For example, on Debian 8, Patch Manager only identifies upgrades that are part of `debian-security jessie`\. On Debian 9, only upgrades that are part of `debian-security stretch` are identified\.

**Note**  
On Debian 8 only: Because some Debian 8\.\* instances refer to an obsolete package repository \(`jessie-backports`\), Patch Manager performs additional steps to ensure that patching operations succeed\. For more information, see [How patches are installed](patch-manager-how-it-works-installation.md)\.

------
#### [ Oracle Linux ]

On Oracle Linux, the Systems Manager patch baseline service uses preconfigured repositories \(repos\) on the instance\. There are usually two preconfigured repos on an instance:
+ **Repo ID**: ol7\_UEKR5/x86\_64

  **Repo name**: Latest Unbreakable Enterprise Kernel Release 5 for Oracle Linux 7Server \(x86\_64\)
+ **Repo ID**: ol7\_latest/x86\_64

  **Repo name**: Oracle Linux 7Server Latest \(x86\_64\) 

**Note**  
All updates are downloaded from the remote repos configured on the instance\. Therefore, the instance must be able to connect to the repos so the patching can be performed\.

Oracle Linux instances use Yum as the package manager, and Yum uses the concept of an update notice as a file named `updateinfo.xml`\. An update notice is simply a collection of packages that fix specific problems\. All packages that are in an update notice are considered Security by Patch Manager\. Individual packages are not assigned classifications or severity levels\. For this reason, Patch Manager assigns the attributes of an update notice to the related packages\.

**Note**  
If you select the **Approved patches include non\-security updates** check box in the **Create patch baseline** page, then packages that are not classified in an `updateinfo.xml` file \(or a package that contains a file without properly formatted Classification, Severity, and Date values\) can be included in the prefiltered list of patches\. However, in order for a patch to be applied, the patch must still meet the user\-specified patch baseline rules\.

------
#### [ RHEL ]

On Red Hat Enterprise Linux, the Systems Manager patch baseline service uses preconfigured repositories \(repos\) on the instance\. There are usually three preconfigured repos on an instance\.

All updates are downloaded from the remote repos configured on the instance\. Therefore, the instance must be able to connect to the repos so the patching can be performed\.

**Note**  
If you select the **Approved patches include non\-security updates** check box in the **Create patch baseline** page, then packages that are not classified in an `updateinfo.xml` file \(or a package that contains a file without properly formatted Classification, Severity, and Date values\) can be included in the prefiltered list of patches\. However, in order for a patch to be applied, the patch must still meet the user\-specified patch baseline rules\.

Red Hat Enterprise Linux 7 instances use Yum as the package manager\. Red Hat Enterprise Linux 8 instances use DNF as the package manager\. Both package managers use the concept of an update notice as a file named `updateinfo.xml`\. An update notice is simply a collection of packages that fix specific problems\. All packages that are in an update notice are considered Security by Patch Manager\. Individual packages are not assigned classifications or severity levels\. For this reason, Patch Manager assigns the attributes of an update notice to the related packages\.

Note that repo locations differ between RHEL 7 and RHEL 8:

RHEL 7  
The following repo IDs are associated with RHUI 2\. RHUI 3 launched in December 2019 and introduced a different naming scheme for Yum repository IDs\. Depending on the RHEL\-7 AMI you create your instances from, you might need to update your commands\. For more information, see [Repository IDs for RHEL 7 in AWS Have Changed](https://access.redhat.com/articles/4599971) on the *Red Hat Customer Portal*\.
+ **Repo ID**: rhui\-REGION\-client\-config\-server\-7/x86\_64

  **Repo name**: Red Hat Update Infrastructure 2\.0 Client Configuration Server 7
+ **Repo ID**: rhui\-REGION\-rhel\-server\-releases/7Server/x86\_64

  **Repo name**: Red Hat Enterprise Linux Server 7 \(RPMs\)
+ **Repo ID**: rhui\-REGION\-rhel\-server\-rh\-common/7Server/x86\_64

  **Repo name**: Red Hat Enterprise Linux Server 7 RH Common \(RPMs\)

RHEL 8  
+ **Repo ID**: rhel\-8\-appstream\-rhui\-rpms

  **Repo name**: Red Hat Enterprise Linux 8 for x86\_64 \- AppStream from RHUI \(RPMs\)
+ **Repo ID**: rhel\-8\-baseos\-rhui\-rpms

  **Repo name**: Red Hat Enterprise Linux 8 for x86\_64 \- BaseOS from RHUI \(RPMs\)
+ **Repo ID**: rhui\-client\-config\-server\-8

  **Repo name**: Red Hat Update Infrastructure 3 Client Configuration Server 8

------
#### [ SLES ]

On SUSE Linux Enterprise Server \(SLES\) instances, the ZYPP library gets the list of available patches \(a collection of packages\) from the following locations:
+ List of repositories: `etc/zypp/repos.d/*`
+ Package information: `/var/cache/zypp/raw/*`

SLES instances use Zypper as the package manager, and Zypper uses the concept of a patch\. A patch is simply a collection of packages that fix a specific problem\. Patch Manager handles all packages referenced in a patch as security\-related\. Because individual packages aren't given classifications or severity, Patch Manager assigns the packages the attributes of the patch that they belong to\.

------
#### [ Ubuntu Server ]

On Ubuntu Server, the Systems Manager patch baseline service uses preconfigured repositories \(repos\) on the instance\. These preconfigured repos are used to pull an updated list of available package upgrades\. For this, Systems Manager performs the equivalent of a `sudo apt-get update` command\. 

Packages are then filtered from `codename-security` repos, where the codename is unique to the release version, such as `trusty` for Ubuntu Server 14\. Patch Manager only identifies upgrades that are part of these repos: 
+ Ubuntu Server 14: `trusty-security`
+ Ubuntu Server 16: `xenial-security`
+ Ubuntu Server 18: `bionic-security`
+ Ubuntu Server 20: `focal-security`

------
#### [ Windows ]

On Microsoft Windows operating systems, Patch Manager retrieves a list of available updates that Microsoft publishes to Microsoft Update and are automatically available to Windows Server Update Services \(WSUS\)\.

Patch Manager continuously monitors for new updates in every AWS Region\. The list of available updates is refreshed in each Region at least once per day\. When the patch information from Microsoft is processed, Patch Manager removes updates that have been replaced by later updates from its patch list \. Therefore, only the most recent update is displayed and made available for installation\. For example, if `KB4012214` replaces `KB3135456`, only `KB4012214` is made available as an update in Patch Manager\.

**Note**  
Patch Manager only makes available patches for Windows Server operating system versions that are supported for Patch Manager\. For example, Patch Manager can't be used to patch Windows RT\.

------