# Step 1: Install or upgrade AWS command line tools<a name="getting-started-cli"></a>

This topic is for users who have *programmatic access* to use AWS Systems Manager \(or any other AWS service\), and who want to run AWS Command Line Interface \(AWS CLI\) or AWS Tools for Windows PowerShell commands from their local machines\. \(Programmatic access and *console access* are different permissions that can be granted to a user account by an AWS account administrator\. A user can be granted one or both access types\. For information, see [ Create non\-Admin IAM users and groups for Systems Manager](setup-create-iam-user.md)\.\)

**Tip**  
As an alternative to running commands from your local machine, you can use AWS CloudShell\. CloudShell is a browser\-based, pre\-authenticated shell that you can launch directly from the AWS Management Console\. You can run AWS CLI commands against AWS services using your preferred shell \(Bash, PowerShell, or Z shell\)\. And you can do this without needing to download or install command line tools\. For more information, see the [https://docs.aws.amazon.com/cloudshell/latest/userguide/](https://docs.aws.amazon.com/cloudshell/latest/userguide/)\.

**Topics**
+ [Installing or upgrading and then configuring the AWS CLI](#getting-started-aws-cli)
+ [Installing or upgrading and then configuring the AWS Tools for PowerShell](#getting-started-twp)

## Installing or upgrading and then configuring the AWS CLI<a name="getting-started-aws-cli"></a>

The AWS CLI is an open source tool that allows you to interact with AWS services using commands in your command\-line shell\. With minimal configuration, the AWS CLI allows you to start running commands that implement functionality equivalent to that provided by the browser\-based AWS Management Console from the command prompt in your terminal program\.

For more information about the AWS CLI, see the *[AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)*\.

For information about all Systems Manager commands you can run using the AWS CLI, see [the Systems Manager section of the *AWS CLI Command Reference*](https://docs.aws.amazon.com/cli/latest/reference/ssm/index.html)\.

**Important**  
Starting January 10th, 2020, AWS CLI version 1\.17 and later no longer support Python 2\.6 or Python 3\.3\. Since that date, the installer for the AWS CLI requires Python 2\.7, Python 3\.4, or a later version\.

**To install or upgrade and then configure the AWS CLI**

1. Follow the instructions in [Installing the AWS Command Line Interface version 2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) in the *AWS Command Line Interface User Guide* to install or upgrade the AWS CLI on your local machine\.
**Tip**  
The AWS CLI is frequently updated with new functionality\. Upgrade \(reinstall\) the AWS CLI periodically to verify that you have access to all the latest functionality\.

1. To configure the AWS CLI, see [Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html) in the *AWS Command Line Interface User Guide*\.

   In this step, you specify credentials that an AWS administrator in your organization has given you, in the following format\.

   ```
   AWS Access Key ID: AKIAIOSFODNN7EXAMPLE
   AWS Secret Access Key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   ```
**Important**  
When you configure the AWS CLI, you're prompted to specify an AWS Region\. Choose one of the supported Regions listed for Systems Manager in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\. If necessary, first verify with an administrator for your AWS account which AWS Region you should choose\.

   For more information about access keys, see [Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *IAM User Guide*\.

1. To verify the installation or upgrade, run the following command from the AWS CLI\.

   ```
   aws ssm help
   ```

   If successful, this command displays a list of available Systems Manager commands\.

## Installing or upgrading and then configuring the AWS Tools for PowerShell<a name="getting-started-twp"></a>

The AWS Tools for PowerShell are a set of PowerShell modules that are built on the functionality exposed by the AWS SDK for \.NET\. The AWS Tools for PowerShell allow you to script operations on your AWS resources from the PowerShell command line\. The cmdlets provide an idiomatic PowerShell experience for specifying parameters and handling results even though they are implemented using the various AWS service HTTP query APIs\.

 For information about the Tools for Windows PowerShell, see the [https://docs.aws.amazon.com/powershell/latest/userguide/](https://docs.aws.amazon.com/powershell/latest/userguide/)\.

 For information about all Systems Manager commands you can run using the AWS Tools for PowerShell, see [the Systems Manager section of the *AWS Tools for PowerShell Cmdlet Reference*](https://docs.aws.amazon.com/powershell/latest/reference/items/SimpleSystemsManagement_cmdlets.html)\.

**To install or upgrade and then configure the AWS Tools for PowerShell**

1. Follow the instructions in [Installing the Tools for PowerShell](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up.html) in the *AWS Tools for Windows PowerShell User Guide* to install or upgrade Tools for PowerShell on your local machine\.
**Tip**  
Tools for PowerShell is frequently updated with new functionality\. Upgrade \(reinstall\) the Tools for PowerShell periodically to ensure that you have access to all the latest functionality\.

1. To configure Tools for PowerShell, see [Using AWS Credentials](https://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html) in the *AWS Tools for Windows PowerShell User Guide*\.

   In this step, you specify credentials that an AWS administrator in your organization has given you, using the following command\.

   ```
   Set-AWSCredential `
   -AccessKey AKIAIOSFODNN7EXAMPLE `
   -SecretKey wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY `
   -StoreAs MyProfileName
   ```
**Important**  
When you configure Tools for PowerShell, you can run `Set-DefaultAWSRegion` to specify an AWS Region\. Choose one of the supported Regions listed for Systems Manager in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\. If necessary, first verify with an administrator for your AWS account which Region you should choose\.

   For more information about access keys, see [Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *IAM User Guide*\.

1. To verify the installation or upgrade, run the following command from Tools for PowerShell\.

   ```
   Get-AWSCmdletName -Service SSM
   ```

   If successful, this command displays a list of available Systems Manager cmdlets\.