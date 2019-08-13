# Learn More About Distributor<a name="what-is-distributor"></a>

See the following topics to learn more about AWS Systems Manager Distributor\.

**Topics**
+ [How Can Distributor Benefit my Organization?](#distributor-benefits)
+ [Who Should Use Distributor?](#distributor-who)
+ [What Are the Features of Distributor?](#distributor-features)
+ [What Is a Package?](#what-is-a-package)

## How Can Distributor Benefit my Organization?<a name="distributor-benefits"></a>

Distributor offers these benefits:
+ **One package, many platforms**

  One document can have attached ZIP files that are installed on different operating systems \(such as Windows, Ubuntu Server, Debian, or Red Hat Enterprise Linux\)\. For more information about supported platforms, see [Supported Package Platforms and Architectures](#what-is-a-package-platforms)\.
+ **Control package access across groups of managed instances**

  You can use Run Command or State Manager to control which of your managed instances get a package and which version of that package\. Managed instances can be grouped by instance IDs, AWS account numbers, tags, or AWS Regions\. You can use State Manager associations to deliver different versions of a package to different groups of instances\.
+ **Many AWS agent packages included and ready to use**

  Distributor includes many AWS agent packages that are ready for you to deploy to managed instances\. Look for packages in the Distributor **Packages** list page that are published by **Amazon**\. Examples include **AmazonCloudWatchAgent** and **AmazonEC2HibernateAgent**\.
+ **Automate deployment **

  To keep your environment current, use State Manager to schedule packages for automatic deployment on target instances when those instances are first launched\.

## Who Should Use Distributor?<a name="distributor-who"></a>
+ Any AWS customer who wants to create new or deploy existing software packages, including AWS\-published packages, to multiple AWS Systems Manager managed instances at one time\.
+ Software developers who create software packages\.
+ Administrators who are responsible for keeping AWS Systems Manager managed instances current with the most up\-to\-date software packages\.

## What Are the Features of Distributor?<a name="distributor-features"></a>
+ **Deployment of packages to both Windows and Linux instances**

  Distributor lets you deploy software packages to Amazon EC2 Windows and Linux instances\. For a list of supported instance operating system types, see [Supported Package Platforms and Architectures](#what-is-a-package-platforms)\.
+ **Deploy packages one time, or on an automated schedule**

  You can choose to deploy packages one time, on a regular schedule, or whenever the default package version is changed to a different version\.
+ **Console, CLI, PowerShell, and SDK access to Distributor capabilities**

  You can work with Distributor by using the AWS Systems Manager console, AWS CLI, AWS Tools for PowerShell, or the AWS SDK of your choice\.
+ **IAM access control**

  By using IAM policies, you can control which members of your organization can create, update, deploy, or delete packages or package versions\. For example, you might want to give an administrator permissions to deploy packages, but not to change packages or create new package versions\.
+ **Logging and auditing capability support**

  You can audit and log Distributor user actions in your AWS account through integration with other AWS services\. For more information, see [Auditing and Logging Distributor Activity](distributor-logging-auditing.md)\.

## What Is a Package?<a name="what-is-a-package"></a>

A *package* is a collection of installable software or assets that includes the following\.
+ A ZIP file of software per target operating system platform\. Each ZIP file must include the following\.
  + An install and an uninstall script\. Windows\-based instances require PowerShell scripts \(scripts named `install.ps1` and `uninstall.ps1`\)\. Linux\-based instances require shell scripts \(scripts named `install.sh` and `uninstall.sh`\)\. SSM Agent reads and carries out the instructions in the install and uninstall scripts\.
  + An executable file\. SSM Agent must find this executable to install the package on target instances\.
+ A JSON\-formatted manifest file that describes the package contents\. The manifest is not included in the ZIP file, but it is stored in the same Amazon S3 bucket as the ZIP files that form the package\. The manifest identifies the package version and maps the ZIP files in the package to target instance attributes, such as operating system version or architecture\. For information about how to create the manifest, see [Step 2: Create the JSON Package Manifest](distributor-working-with-packages-create.md#packages-manifest)\.

When you choose **Simple** package creation in the Distributor console, Distributor generates the installation and uninstallation scripts, file hashes, and the JSON package manifest for you, based on the software executable file name and target platforms and architectures\.

### Supported Package Platforms and Architectures<a name="what-is-a-package-platforms"></a>

Distributor supports package distribution to any release version of the following platforms that is supported as a Systems Manager managed instance\. A version value must match the exact release version of the operating system AMI that you are targeting\. For more information about determining this version, see step 4 of [Step 2: Create the JSON Package Manifest](distributor-working-with-packages-create.md#packages-manifest)\.


| Platform | Code Value in Manifest File | Architecture | 
| --- | --- | --- | 
|  Windows Server  |  `windows`  |  `x86_64` or `386`  | 
|  Debian  |  `debian`  |  `x86_64` or `386`  | 
|  Ubuntu Server  |  `ubuntu`  |  `x86_64` or `386` `arm64` \(Ubuntu Server 16 and later, A1 instance types\)  | 
|  Red Hat Enterprise Linux \(RHEL\)  |  `redhat`  |  `x86_64` or `386` `arm64` \(RHEL 7\.6 and later, A1 instance types\)  | 
|  CentOS  |  `centos`  |  `x86_64` or `386`  | 
|  Amazon Linux and Amazon Linux 2  |  `amazon`  |  `x86_64` or `386` `arm64` \(Amazon Linux 2, A1 instance types\)  | 
|  SUSE Linux Enterprise Server \(SLES\)  |  `suse`  |  `x86_64` or `386`  | 
|  openSUSE  |  `opensuse`  |  `x86_64` or `386`  | 
|  openSUSE Leap  |  `opensuseleap`  |  `x86_64` or `386`  | 