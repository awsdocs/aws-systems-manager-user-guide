# Step 1: Install or upgrade AWS command line tools<a name="getting-started-cli"></a>

This topic is for users who have *programmatic access* to use Systems Manager \(or any other AWS service\), and who want to run AWS CLI or AWS Tools for Windows PowerShell commands from their local machines\. 

**Note**  
Programmatic access and *console access* are different permissions that can be granted to a user account by an AWS account administrator\. A user can be granted one or both access types\. For information, see [ Create non\-Admin IAM users and groups for Systems Manager](setup-create-iam-user.md)\.

For information about the AWS CLI, see the *[AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)*\. For information about the AWS Tools for Windows PowerShell, see the *[AWS Tools for Windows PowerShell User Guide](https://docs.aws.amazon.com/powershell/latest/userguide/)*\.

For information about all Systems Manager commands you can run using the AWS CLI, see [the Systems Manager section of the *AWS CLI Command Reference*](https://docs.aws.amazon.com/cli/latest/reference/ssm/index.html)\. For information about all Systems Manager commands you can run using the AWS Tools for PowerShell, see [the Systems Manager section of the *AWS Tools for PowerShell Cmdlet Reference*](https://docs.aws.amazon.com/powershell/latest/reference/items/AWS_Systems_Manager_cmdlets.html)\.

**Important**  
Beginning January 10th, 2020, AWS CLI version 1\.17 and later will no longer support Python 2\.6 or Python 3\.3\. After this date, the installer for the AWS CLI will require Python 2\.7, Python 3\.4, or a later version to successfully install the AWS CLI\. For more information, see [Using the AWS CLI version 1 with Python 2\.6 or Python 3\.3](https://docs.aws.amazon.com/cli/latest/userguide/deprecate-python-26-33.html) in the *IAM User Guide*\.

------
#### [ AWS CLI ]

**To install or upgrade and then configure the AWS CLI**

1. Follow the instructions in [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html) in the *AWS Command Line Interface User Guide* to install or upgrade the AWS CLI on your local machine\.
**Tip**  
The AWS CLI is frequently updated with new functionality\. Upgrade \(reinstall\) the CLI periodically to ensure that you have access to all the latest functionality\.

1. To configure the AWS CLI, see [Configuring the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.html) in the *AWS Command Line Interface User Guide*\.

   In this step, you specify credentials that an AWS administrator in your organization has given you, in the following format:

   ```
   AWS Access Key ID: AKIAIOSFODNN7EXAMPLE
   AWS Secret Access Key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   ```
**Important**  
When you configure the AWS CLI, you are prompted to specify an AWS Region\. Choose one of the supported Regions listed for Systems Manager in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\. If necessary, first verify with an administrator for your AWS account which Region you should choose\.

   For more information about access keys, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html) in the *IAM User Guide*

1. To verify the installation or upgrade, run the following command from the AWS CLI:

   ```
   aws ssm help
   ```

   If successful, this command displays a list of available Systems Manager commands\.

------
#### [ AWS Tools for PowerShell ]

**To install or upgrade and then configure the AWS Tools for Windows PowerShell**

1. Follow the instructions in [Setting up the AWS Tools for Windows PowerShell or AWS Tools for PowerShell Core](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up.html) in the *AWS Tools for Windows PowerShell User Guide* to install or upgrade AWS Tools for PowerShell on your local machine\.
**Tip**  
AWS Tools for PowerShell is frequently updated with new functionality\. Upgrade \(reinstall\) the AWS Tools for PowerShell periodically to ensure that you have access to all the latest functionality\.

1. To configure AWS Tools for PowerShell, see [Using AWS Credentials](https://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html) in the *AWS Tools for Windows PowerShell User Guide*\.

   In this step, you specify credentials that an AWS administrator in your organization has given you, using the following command\.

   ```
   Set-AWSCredential `
   -AccessKey AKIAIOSFODNN7EXAMPLE `
   -SecretKey wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY `
   -StoreAs MyProfileName
   ```
**Important**  
When you configure AWS Tools for PowerShell, you can run `Set-DefaultAWSRegion` to specify an AWS Region\. Choose one of the supported Regions listed for Systems Manager in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\. If necessary, first verify with an administrator for your AWS account which Region you should choose\.

   For more information about access keys, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html) in the *IAM User Guide*\.

1. To verify the installation or upgrade, run the following command from AWS Tools for PowerShell\.

   ```
   Get-AWSCmdletName -Service SSM
   ```

   If successful, this command displays a list of available Systems Manager cmdlets\.

------