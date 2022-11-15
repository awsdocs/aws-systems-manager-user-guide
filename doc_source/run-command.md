# Running commands on managed nodes<a name="run-command"></a>

This section includes information about how to send commands from the AWS Systems Manager console to managed nodes\. This section also includes information about how to cancel a command\.

For information about how to send commands using Windows PowerShell, see [Walkthrough: Use the AWS Tools for Windows PowerShell with Run Command](walkthrough-powershell.md) or the examples in the [AWS Systems Manager section of the AWS Tools for PowerShell Cmdlet Reference](https://docs.aws.amazon.com/powershell/latest/reference/items/AWS_Systems_Manager_cmdlets.html)\. For information about how to send commands using the AWS Command Line Interface \(AWS CLI\), see the [Walkthrough: Use the AWS CLI with Run Command](walkthrough-cli.md) or the examples in the [SSM CLI Reference](https://docs.aws.amazon.com/cli/latest/reference/ssm/)\.

**Important**  
When you send a command using Run Command, don't include sensitive information formatted as plaintext, such as passwords, configuration data, or other secrets\. All Systems Manager API activity in your account is logged in an Amazon S3 bucket for AWS CloudTrail logs\. This means that any user with access to S3 bucket can view the plaintext values of those secrets\. For this reason, we recommend creating and using `SecureString` parameters to encrypt sensitive data you use in your Systems Manager operations\.  
For more information, see [Restricting access to Systems Manager parameters using IAM policies](sysman-paramstore-access.md)\.

**Topics**
+ [Running commands from the console](rc-console.md)
+ [Running commands using a specific document version](run-command-version.md)
+ [Run commands at scale](send-commands-multiple.md)
+ [Canceling a command](rc-cancel.md)