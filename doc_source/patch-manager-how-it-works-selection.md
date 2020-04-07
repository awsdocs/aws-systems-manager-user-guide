# How Security Patches Are Selected<a name="patch-manager-how-it-works-selection"></a>

The primary focus of Patch Manager is on installing operating systems security\-related updates on instances\. By default, Patch Manager doesn't install all available patches, but rather a smaller set of patches focused on security\.

**Note**  
On all Linux\-based systems supported by Patch Manager, you can choose a different source repository configured for the instance, typically to install nonsecurity updates\. For information, see [How to Specify an Alternative Patch Source Repository \(Linux\)](patch-manager-how-it-works-alt-source-repository.md)\.

Choose from the following tabs to learn how Patch Manager selects security patches for your operating system\.

------
#### [ Windows ]

On Microsoft Windows operating systems, Patch Manager retrieves a list of available updates that Microsoft publishes through its update services, such as Microsoft Update and Windows Server Update Services \(WSUS\)\. Patch Manager continuously monitors for new updates in every AWS Region\. The list of available updates is refreshed in each Region at least once per day\. When the patch information from Microsoft is processed, Patch Manager removes updates that have been replaced by later updates from its patch list \. Therefore, only the most recent update is displayed and made available for installation\. For example, if `KB4012214` replaces `KB3135456`, only `KB4012214` is made available as an update in Patch Manager\.

**Note**  
Patch Manager only makes available patches for Windows Server operating system versions that are supported for Patch Manager\. For example, Patch Manager can't be used to patch Windows RT\.

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
#### [ RHEL ]

On Red Hat Enterprise Linux, the Systems Manager patch baseline service uses preconfigured repositories \(repos\) on the instance\. There are usually three preconfigured repos on an instance:
+ **Repo ID**: rhui\-REGION\-client\-config\-server\-7/x86\_64

  **Repo name**: Red Hat Update Infrastructure 2\.0 Client Configuration Server 7
+ **Repo ID**: rhui\-REGION\-rhel\-server\-releases/7Server/x86\_64

  **Repo name**: Red Hat Enterprise Linux Server 7 \(RPMs\)
+ **Repo ID**: rhui\-REGION\-rhel\-server\-rh\-common/7Server/x86\_64

  **Repo name**: Red Hat Enterprise Linux Server 7 RH Common \(RPMs\)

**Note**  
All updates are downloaded from the remote repos configured on the instance\. Therefore, the instance must be able to connect to the repos so the patching can be performed\.

Red Hat Enterprise Linux instances use Yum as the package manager, and Yum uses the concept of an update notice as a file named `updateinfo.xml`\. An update notice is simply a collection of packages that fix specific problems\. All packages that are in an update notice are considered Security by Patch Manager\. Individual packages are not assigned classifications or severity levels\. For this reason, Patch Manager assigns the attributes of an update notice to the related packages\.

**Note**  
If you select the **Approved patches include non\-security updates** check box in the **Create patch baseline** page, then packages that are not classified in an `updateinfo.xml` file \(or a package that contains a file without properly formatted Classification, Severity, and Date values\) can be included in the prefiltered list of patches\. However, in order for a patch to be applied, the patch must still meet the user\-specified patch baseline rules\.

------
#### [ Ubuntu ]

On Ubuntu Server, the Systems Manager patch baseline service uses preconfigured repositories \(repos\) on the instance\. These preconfigured repos are used to pull an updated list of available package upgrades\. For this, Systems Manager performs the equivalent of a `sudo apt-get update` command\. 

Packages are then filtered from `codename-security` repos, where the codename is something like `trusty` or `xenial`\. For example, on Ubuntu Server 14, Patch Manager only identifies upgrades that are part of `trusty-security`\. On Ubuntu Server 16, only upgrades that are part of `xenial-security` are identified\.

------
#### [ SLES ]

On SUSE Linux Enterprise Server \(SLES\) instances, the ZYPP library gets the list of available patches \(a collection of packages\) from the following locations:
+ List of repositories: `etc/zypp/repos.d/*`
+ Package information: `/var/cache/zypp/raw/*`

SLES instances use Zypper as the package manager, and Zypper uses the concept of a patch\. A patch is simply a collection of packages that fix a specific problem\. Patch Manager handles all packages referenced in a patch as security\-related\. Because individual packages aren't given classifications or severity, Patch Manager assigns the packages the attributes of the patch that they belong to\.

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

CentOS instances use Yum as the package manager, and Yum uses the concept of an update notice\. An update notice is simply a collection of packages that fix specific problems\. All packages that are in an update notice are considered Security packages by Patch Manager\.

However, CentOS default repos aren't configured with an update notice\. This means that Patch Manager does not detect packages on a default CentOS repo\. To enable Patch Manager to process packages that aren't contained in an update notice, you must enable the `EnableNonSecurity` flag in the patch baseline rules\.

**Note**  
CentOS update notices are supported\. Repos with update notices can be downloaded after launch\.

------