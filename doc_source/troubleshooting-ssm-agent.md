# Troubleshooting SSM Agent<a name="troubleshooting-ssm-agent"></a>

If you experience problems running operations on your managed instances, there might be a problem with SSM Agent\. Use the following information to help you view SSM Agent log files and troubleshoot the agent\. 

**Topics**
+ [SSM Agent is out of date](#ssm-agent-out-of-date)
+ [View SSM Agent log files](#systems-manager-ssm-agent-log-files)

## SSM Agent is out of date<a name="ssm-agent-out-of-date"></a>

An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

## View SSM Agent log files<a name="systems-manager-ssm-agent-log-files"></a>

SSM Agent logs information in the following files\. The information in these files can also help you troubleshoot problems\.

**Note**  
If you choose to view these logs by using Windows File Explorer, be sure to enable the viewing of hidden files and system files in Folder Options\.

**On Windows**
+ `%PROGRAMDATA%\Amazon\SSM\Logs\amazon-ssm-agent.log`
+ `%PROGRAMDATA%\Amazon\SSM\Logs\errors.log`

**On Linux**
+ `/var/log/amazon/ssm/amazon-ssm-agent.log`
+ `/var/log/amazon/ssm/errors.log`