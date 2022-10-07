# Step 2: Create an Admin IAM user for AWS<a name="setup-create-admin-user"></a>

  When you create an AWS account, you begin with one sign\-in identity that has complete access to all AWS services and resources in the account\. This identity is called the AWS account *root user* and is accessed by signing in with the email address and password that you used to create the account\. We strongly recommend that you do not use the root user for your everyday tasks\. Safeguard your root user credentials and use them to perform the tasks that only the root user can perform\. For the complete list of tasks that require you to sign in as the root user, see [Tasks that require root user credentials](https://docs.aws.amazon.com/general/latest/gr/root-vs-iam.html#aws_tasks-that-require-root) in the *AWS General Reference*\. 

In this procedure, you use the AWS account root user to create your first user in AWS Identity and Access Management \(IAM\)\. You add this IAM user to an Administrators group, to ensure that you have access to all services and their resources in your account\. The next time that you access your AWS account, you should sign in with the credentials for this IAM user\. As a best practice, create only the credentials that the user needs\. For example, for a user who requires access only through the AWS Management Console, do not create access keys\. Optionally, you can configure [multi\-factor authentication \(MFA\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) for the user\. MFA requires the user to provide a one\-time\-use code each time he or she signs into the AWS Management Console\.

To create an IAM user with restricted permissions, see [Step 3: Create non\-Admin IAM users and groups for Systems Manager](setup-create-iam-user.md)\.

To create an administrator user, choose one of the following options\.


****  

| Choose one way to manage your administrator | To | By | You can also | 
| --- | --- | --- | --- | 
| In IAM Identity Center \(Recommended\) | Use short\-term credentials to access AWS\.This aligns with the security best practices\. For information about best practices, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#bp-users-federation-idp) in the *IAM User Guide*\. | Following the instructions in [Getting started](https://docs.aws.amazon.com/singlesignon/latest/userguide/getting-started.html) in the AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide\. | Configure programmatic access by [Configuring the AWS CLI to use AWS IAM Identity Center \(successor to AWS Single Sign\-On\)](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html) in the AWS Command Line Interface User Guide\. | 
| In IAM \(Not recommended\) | Use long\-term credentials to access AWS\. | Following the instructions in [Creating your first IAM admin user and user group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the IAM User Guide\. | Configure programmatic access by [Managing access keys for IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the IAM User Guide\. | 

Continue to [Step 3: Create non\-Admin IAM users and groups for Systems Manager](setup-create-iam-user.md)\.