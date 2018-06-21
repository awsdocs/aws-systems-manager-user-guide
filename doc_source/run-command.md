# Running Commands Using Systems Manager Run Command<a name="run-command"></a>

This section includes information about how to send commands from the Amazon EC2 console, and how to send commands to a fleet of instances by using the `Targets` parameter with EC2 tags\. This section also includes information about how to cancel a command\.

For information about how to send commands using Windows PowerShell, see [Walkthrough: Use the AWS Tools for Windows PowerShell with Run Command](walkthrough-powershell.md) or the examples in the [Tools for Windows PowerShell Reference](http://docs.aws.amazon.com/powershell/latest/reference/items/Amazon_Simple_Systems_Management_cmdlets.html)\. For information about how to send commands using the AWS CLI, see the [Walkthrough: Use the AWS CLI with Run Command](walkthrough-cli.md) or the examples in the [SSM CLI Reference](http://docs.aws.amazon.com/cli/latest/reference/ssm/index.html)\.

**Important**  
If this is your first time using Run Command, we recommend executing commands against a test instance or an instance that is not being used in a production environment\.

**Topics**
+ [Running Commands from the Console](rc-console.md)
+ [Sending Commands that Use the Document Version Parameter](run-command-version.md)
+ [Sending Commands to a Fleet](send-commands-multiple.md)
+ [Rebooting Managed Instance from Scripts](send-commands-reboot.md)
+ [Canceling a Command](rc-cancel.md)