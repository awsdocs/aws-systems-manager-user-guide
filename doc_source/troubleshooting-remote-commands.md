# Troubleshooting Systems Manager Run Command<a name="troubleshooting-remote-commands"></a>

Run Command, a capability of AWS Systems Manager, provides status details with each command execution\. For more information about the details of command statuses, see [Understanding command statuses](monitor-commands.md)\. You can also use the information in this topic to help troubleshoot problems with Run Command\.

**Topics**
+ [Some of my managed nodes are missing](#where-are-instances)
+ [A step in my script failed, but the overall status is 'succeeded'](#ts-exit-codes)
+ [SSM Agent isn't running properly](#ts-ssmagent-linux)

## Some of my managed nodes are missing<a name="where-are-instances"></a>

In the **Run a command** page, after you choose an SSM document to run and select **Manually selecting instances** in the **Targets** section, a list is displayed of managed nodes you can choose to run the command on\.

If a managed node you expect to see isn't listed, see [Troubleshooting managed node availability](troubleshooting-managed-instances.md) for troubleshooting tips\.

After you create, activate, reboot, or restart a managed node, install Run Command on a node, or attach an AWS Identity and Access Management \(IAM\) instance profile to a node, it can take a few minutes for the managed node to be added to the list\.

## A step in my script failed, but the overall status is 'succeeded'<a name="ts-exit-codes"></a>

Using Run Command, you can define how your scripts handle exit codes\. By default, the exit code of the last command run in a script is reported as the exit code for the entire script\. You can, however, include a conditional statement to exit the script if any command before the final one fails\. For information and examples, see [Managing exit codes in Run Command commands](command-exit-codes.md)\. 

## SSM Agent isn't running properly<a name="ts-ssmagent-linux"></a>

If you experience problems running commands using Run Command, there might be a problem with the SSM Agent\. For information about investigating issues with SSM Agent, see [Troubleshooting SSM Agent](troubleshooting-ssm-agent.md)\. 