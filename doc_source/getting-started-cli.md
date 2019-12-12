# Step 1: Install or Upgrade the AWS CLI<a name="getting-started-cli"></a>

This topic is for users who have *programmatic access* to use Systems Manager \(or any other AWS service\), and who want to run AWS CLI commands from their local machines\. 

**Note**  
Programmatic access and *console access* are different permissions that can be granted to a user account by an AWS account administrator\. A user can be granted one or both access types\. For information, see [ Create Non\-Admin IAM Users and Groups for Systems Manager](setup-create-iam-user.md)\.

For information about the AWS CLI, see the *[AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)*\.

For information about all Systems Manager commands you can run using the AWS CLI, see [the Systems Manager section of the *AWS CLI Command Reference*](https://docs.aws.amazon.com/cli/latest/reference/ssm/index.html)\.

**Important**  
Beginning January 10th, 2020, AWS CLI version 1\.17 and later will no longer support Python 2\.6 or Python 3\.3\. After this date, the installer for the AWS CLI will require Python 2\.7, Python 3\.4, or a later version to successfully install the AWS CLI\. For more information, see [Using the AWS CLI version 1 with Python 2\.6 or Python 3\.3](https://docs.aws.amazon.com/cli/latest/userguide/deprecate-python-26-33.html) in the *IAM User Guide*\.

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
When you configure the AWS CLI, you are prompted to specify an AWS Region\. Choose one of the supported Regions listed for Systems Manager in [Systems Manager Service Endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\. If necessary, first verify with an administrator for your AWS account which Region you should choose\.

   For more information about access keys, see [Managing Access Keys for IAM Users](https://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingCredentials.html) in the *IAM User Guide*

1. To verify the installation or upgrade, run the following command from the AWS CLI:

   ```
   aws ssm help
   ```

   If successful, this command displays a list of available Systems Manager commands\.