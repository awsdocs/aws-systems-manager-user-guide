# Send Logs to CloudWatch Logs \(SSM Agent\)<a name="monitoring-ssm-agent"></a>

AWS Systems Manager Agent is Amazon software that runs on your Amazon EC2 instances and your hybrid instances that are configured for Systems Manager \(hybrid instances\)\. SSM Agent processes requests from the Systems Manager service in the cloud and configures your machine as specified in the request\. For more information about SSM Agent, see [Installing and Configuring SSM Agent](ssm-agent.md)\.

In addition, following the steps below, you can configure SSM Agent to send log data to Amazon CloudWatch Logs\. 

**Before You Begin**  
Create a log group in Amazon CloudWatch Logs\. For more information, see [Create a Log Group in CloudWatch Logs](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Create-Log-Group.html) in the *Amazon CloudWatch Logs User Guide*\.

**To configure SSM Agent to send logs to CloudWatch**

1. Log into an instance and locate the following file:

   On Windows: `%PROGRAMFILES%\Amazon\SSM\seelog.xml.template`

   On Linux: `/etc/amazon/ssm/seelog.xml.template`

1. Change the file name from `seelog.xml.template` to `seelog.xml`\.

1. Open the `seelog.xml` file in a text editor, and locate the following section:

   ```
   <outputs formatid="fmtinfo">
   		<console formatid="fmtinfo"/>
   		<rollingfile type="size" maxrolls="5" maxsize="30000000" filename="{{LOCALAPPDATA}}\Amazon\SSM\Logs\amazon-ssm-agent.log"/>
   		<filter formatid="fmterror" levels="error,critical">
   		<rollingfile type="size" maxrolls="5" maxsize="10000000" filename="{{LOCALAPPDATA}}\Amazon\SSM\Logs\errors.log"/>
   		</filter>
   	</outputs>
   ```

1. Edit the file, and add the following *custom name* element after the closing </filter> tag, as shown in the following example\.

   ```
   <seelog minlevel="info" critmsgcount="500" maxinterval="100000000" mininterval="2000000" type="adaptive">
   	<exceptions>
   		<exception minlevel="error" filepattern="test*"/>
   	</exceptions>
   	<outputs formatid="fmtinfo">
   		<console formatid="fmtinfo"/>
   		<rollingfile type="size" maxrolls="5" maxsize="30000000" filename="{{LOCALAPPDATA}}\Amazon\SSM\Logs\amazon-ssm-agent.log"/>
   		<filter formatid="fmterror" levels="error,critical">
   		<rollingfile type="size" maxrolls="5" maxsize="10000000" filename="{{LOCALAPPDATA}}\Amazon\SSM\Logs\errors.log"/>
   		</filter>
   		<custom name="cloudwatch_receiver" formatid="fmtdebug" data-log-group="Your CloudWatch Log Group Name"/>
   	</outputs>
   ```

1. Save your changes\.

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. Choose **Logs**, and then choose your log group\. The log stream for SSM Agent log file data is organized by instance ID\.