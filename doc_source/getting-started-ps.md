# Step 2: Install or Upgrade the AWS Tools for PowerShell<a name="getting-started-ps"></a>

This topic is for users who have *programmatic access* to use Systems Manager \(or any other AWS service\), and who want to run AWS Tools for PowerShell commands from their local machines\. 

**Note**  
Programmatic access and *console access* require different permissions that can be granted to a user account by an AWS account administrator\. A user can be granted one or both access types\. For information, see [ Create Non\-Admin IAM Users and Groups for Systems Manager](setup-create-iam-user.md)\.

For information about the AWS Tools for Windows PowerShell, see the *[AWS Tools for Windows PowerShell User Guide](https://docs.aws.amazon.com/powershell/latest/userguide/)*\.

For information about all Systems Manager commands you can run using the AWS Tools for PowerShell, see [the Systems Manager section of the *AWS Tools for PowerShell Cmdlet Reference*](https://docs.aws.amazon.com/powershell/latest/reference/items/AWS_Systems_Manager_cmdlets.html)\.

**To install or upgrade and then configure the AWS Tools for Windows PowerShell**

1. Follow the instructions in [Setting up the AWS Tools for Windows PowerShell or AWS Tools for PowerShell Core](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up.html) in the *AWS Tools for Windows PowerShell User Guide* to install or upgrade AWS Tools for PowerShell on your local machine\.
**Tip**  
AWS Tools for PowerShell is frequently updated with new functionality\. Upgrade \(reinstall\) the AWS Tools for PowerShell periodically to ensure that you have access to all the latest functionality\.

1. To configure AWS Tools for PowerShell, see [Using AWS Credentials](https://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html) in the *AWS Tools for Windows PowerShell User Guide*\.

   In this step, you specify credentials that an AWS administrator in your organization has given you, using the following command\.

   ```
   Set-AWSCredential -AccessKey AKIAIOSFODNN7EXAMPLE -SecretKey wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY -StoreAs MyProfileName
   ```
**Important**  
When you configure AWS Tools for PowerShell, you can run `Set-DefaultAWSRegion` to specify an AWS Region\. Choose one of the supported Regions listed for Systems Manager in [Systems Manager Service Endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\. If necessary, first verify with an administrator for your AWS account which Region you should choose\.

   For more information about access keys, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html) in the *IAM User Guide*\.

1. To verify the installation or upgrade, run the following command from AWS Tools for PowerShell\.

   ```
   Get-AWSCmdletName -Service SSM
   ```

   If successful, this command displays a list of available Systems Manager cmdlets\.