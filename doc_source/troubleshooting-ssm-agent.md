# Troubleshooting SSM Agent<a name="troubleshooting-ssm-agent"></a>

If you experience problems running operations on your managed instances, there might be a problem with AWS Systems Manager Agent \(SSM Agent\)\. Use the following information to help you view SSM Agent log files and troubleshoot the agent\. 

**Topics**
+ [SSM Agent is out of date](#ssm-agent-out-of-date)
+ [View SSM Agent log files](#systems-manager-ssm-agent-log-files)
+ [Agent log files don't rotate \(Windows\)](#systems-manager-ssm-agent-troubleshooting-log-rotation)
+ [Unable to connect to SSM endpoints](#systems-manager-ssm-agent-troubleshooting-endpoint-access)

## SSM Agent is out of date<a name="ssm-agent-out-of-date"></a>

An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. Subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub to get notifications about SSM Agent updates\.

## View SSM Agent log files<a name="systems-manager-ssm-agent-log-files"></a>

SSM Agent logs information in the following files\. The information in these files can also help you troubleshoot problems\.

**Note**  
If you choose to view these logs by using Windows File Explorer, be sure to enable the viewing of hidden files and system files in Folder Options\.

**On Windows**
+ `%PROGRAMDATA%\Amazon\SSM\Logs\amazon-ssm-agent.log`
+ `%PROGRAMDATA%\Amazon\SSM\Logs\errors.log`

**On Linux and macOS**
+ `/var/log/amazon/ssm/amazon-ssm-agent.log`
+ `/var/log/amazon/ssm/errors.log`

For Linux instances, you might find more information in the `messages` file written to the following directory: `/var/log`\.

## Agent log files don't rotate \(Windows\)<a name="systems-manager-ssm-agent-troubleshooting-log-rotation"></a>

If you specify date\-based log file rotation in the seelog\.xml file \(on Windows Server instances\) and the logs don't rotate, then you must specify the `fullname=true` parameter\. Here is an example of a seelog\.xml configuration file with the `fullname=true` parameter specified\.

```
<seelog type="adaptive" mininterval="2000000" maxinterval="100000000" critmsgcount="500" minlevel="debug">
   <exceptions>
      <exception filepattern="test*" minlevel="error" />
   </exceptions>
   <outputs formatid="fmtinfo">
      <console formatid="fmtinfo" />
      <rollingfile type="date" datepattern="200601021504" maxrolls="4" filename="C:\ProgramData\Amazon\SSM\Logs\amazon-ssm-agent.log" fullname=true />
      <filter levels="error,critical" formatid="fmterror">
         <rollingfile type="date" datepattern="200601021504" maxrolls="4" filename="C:\ProgramData\Amazon\SSM\Logs\errors.log" fullname=true />
      </filter>
   </outputs>
   <formats>
      <format id="fmterror" format="%Date %Time %LEVEL [%FuncShort @ %File.%Line] %Msg%n" />
      <format id="fmtdebug" format="%Date %Time %LEVEL [%FuncShort @ %File.%Line] %Msg%n" />
      <format id="fmtinfo" format="%Date %Time %LEVEL %Msg%n" />
   </formats>
</seelog>
```

## Unable to connect to SSM endpoints<a name="systems-manager-ssm-agent-troubleshooting-endpoint-access"></a>

SSM Agent must be able to connect to the following endpoints:
+ `ssm.region.amazonaws.com`
+ `ssmmessages.region.amazonaws.com`
+ `ec2messages.region.amazonaws.com`

SSM Agent won't work if it can't communicate with the above endpoints, even if you use AWS provided Amazon Machine Images \(AMIs\) such as Amazon Linux or Amazon Linux 2\. Your network configuration must have open internet access or you must have custom virtual private cloud \(VPC\) endpoints configured\. If you don't plan on creating a custom VPC endpoint, check your internet gateways or NAT gateways\. For more information about how to manage VPC endpoints, see [Step 6: \(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md)\.