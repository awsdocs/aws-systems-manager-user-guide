# Running Commands Using Systems Manager Run Command<a name="run-command"></a>

This section includes information about how to send commands from the AWS Systems Manager console, and how to send commands to a fleet of instances by using the `Targets` parameter with EC2 tags\. This section also includes information about how to cancel a command\.

For information about how to send commands using Windows PowerShell, see [Walkthrough: Use the AWS Tools for Windows PowerShell with Run Command](walkthrough-powershell.md) or the examples in the [AWS Systems Manager section of the AWS Tools for PowerShell Cmdlet Reference](https://docs.aws.amazon.com/powershell/latest/reference/items/AWS_Systems_Manager_cmdlets.html)\. For information about how to send commands using the AWS CLI, see the [Walkthrough: Use the AWS CLI with Run Command](walkthrough-cli.md) or the examples in the [SSM CLI Reference](https://docs.aws.amazon.com/cli/latest/reference/ssm/index.html)\.

**Important**  
If this is your first time using Run Command, we recommend executing commands against a test instance or an instance that is not being used in a production environment\.

**Topics**
+ [Running Commands from the Console](rc-console.md)
+ [Running PowerShell Scripts on Linux Instances](powershell-run-command-linux.md)
+ [Running Commands Using the Document Version Parameter](run-command-version.md)
+ [Using Targets and Rate Controls](send-commands-multiple.md)
+ [Rebooting Managed Instance from Scripts](send-commands-reboot.md)
+ [Canceling a Command](rc-cancel.md)