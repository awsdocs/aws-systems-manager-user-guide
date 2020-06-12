# Running commands using Systems Manager Run Command<a name="run-command"></a>

This section includes information about how to send commands from the AWS Systems Manager console, and how to send commands to a fleet of instances by using the `Targets` parameter with EC2 tags\. This section also includes information about how to cancel a command\.

**Tip**  
If this is your first time using Run Command, we recommend executing commands against a test instance or an instance that is not being used in a production environment\.

For information about how to send commands using Windows PowerShell, see [Walkthrough: Use the AWS Tools for Windows PowerShell with Run Command](walkthrough-powershell.md) or the examples in the [AWS Systems Manager section of the AWS Tools for PowerShell Cmdlet Reference](https://docs.aws.amazon.com/powershell/latest/reference/items/AWS_Systems_Manager_cmdlets.html)\. For information about how to send commands using the AWS CLI, see the [Walkthrough: Use the AWS CLI with Run Command](walkthrough-cli.md) or the examples in the [SSM CLI Reference](https://docs.aws.amazon.com/cli/latest/reference/ssm/index.html)\.

**Important**  
When you run a command in Run Command, do not include any sensitive information formatted as plaintext, such as passwords, configuration data, or other secrets\. All Systems Manager API activity in your account is logged in an Amazon S3 bucket, in AWS CloudTrail logs\. This means that any user with access to that S3 bucket can view the plaintext values of those secrets\. For this reason, we strongly recommend creating and using `SecureString` parameters to encrypt the sensitive data you use in your Systems Manager operations\.  
For more information, see [SecureString parameters](sysman-paramstore-securestring.md)\.

**Topics**
+ [Running commands from the console](rc-console.md)
+ [Running PowerShell scripts on Linux instances](powershell-run-command-linux.md)
+ [Running commands using the document version parameter](run-command-version.md)
+ [Using targets and rate controls](send-commands-multiple.md)
+ [Canceling a command](rc-cancel.md)