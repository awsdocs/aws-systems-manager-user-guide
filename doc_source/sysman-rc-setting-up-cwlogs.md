# Configuring Amazon CloudWatch Logs for Run Command<a name="sysman-rc-setting-up-cwlogs"></a>

When you send a command by using Run Command, you can specify where you want to send the command output\. By default, Systems Manager returns only the first 1,200 characters of the command output\. If you want to view the full details of the command output, you can specify an Amazon Simple Storage Service \(Amazon S3\) bucket\. Or you can specify Amazon CloudWatch Logs\. If you specify CloudWatch Logs, Run Command periodically sends all command output and error logs to CloudWatch Logs\. You can monitor output logs in near real\-time, search for specific phrases, values, or patterns, and create alarms based on the search\. 

If you configured your instance or on\-premises hybrid machine to use the **AmazonEC2RoleforSSM** AWS Identity and Access Management \(IAM\) managed policy, then your instance requires no additional configuration to send output to CloudWatch Logs\. You simply need to choose this option if sending commands from the console, or add the `cloud-watch-output-config` section and `CloudWatchOutputEnabled` parameter if using the AWS CLI, Tools for Windows PowerShell, or an API action\. The `cloud-watch-output-config` section and `CloudWatchOutputEnabled` parameter are described in more detail later in this topic\.

If you are using a custom policy on your instances, then you must update the policy on each instance to allow Systems Manager to send output and logs to CloudWatch Logs\. Add the following policy objects to your custom policy\. For more information, about updating an IAM policy, see [Editing IAM Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-edit.html)\.

```
{
   "Effect":"Allow",
   "Action":[
      "logs:CreateLogGroup",
      "logs:CreateLogStream",
      "logs:DescribeLogGroups",
      "logs:DescribeLogStreams",
      "logs:PutLogEvents"
   ],
   "Resource":"*"
},
```

## Specifying CloudWatch Logs When You Send Commands<a name="sysman-rc-setting-up-cwlogs-send"></a>

To specify CloudWatch Logs as the output when you send a command from the AWS Management Console, choose **CloudWatch Output** in the **Output options** section\. Optionally, you can specify the name of CloudWatch Logs group where you want to send command output\. If you don't specify a group name, Systems Manager automatically creates a log group for you\. The log group uses the following naming format: aws/ssm/*SystemsManagerDocumentName*\.

If you run commands by using the AWS CLI, then you must specify the `cloud-watch-output-config` section in your command\. This section enables you to specify the `CloudWatchOutputEnabled` parameter, and optionally, the `CloudWatchLogGroupName` parameter\. Here is an example:

```
aws ssm send-command --document-name "AWS-RunPowerShellScript" --parameters commands=["echo helloWorld"] --targets "Key=instanceids,Values=an instance ID” --cloud-watch-output-config '{"CloudWatchLogGroupName":"log group name","CloudWatchOutputEnabled":true}'
```

## Viewing Command Output in CloudWatch Logs<a name="sysman-rc-setting-up-cwlogs-view"></a>

As soon as the command starts to run, Systems Manager sends output to CloudWatch Logs in near real\-time\. The output in CloudWatch Logs uses the following format:

`CommandID/InstanceID/PluginID/stdout` 

`CommandID/InstanceID/PluginID/stderr`

Output from the execution is uploaded every 30 seconds or when the buffer exceeds 200 KB, whichever happens first\.

**Note**  
Log Streams are only created when output data is available\. For example, if there is no error data for an execution, the stderr stream isn't created\.

Here is an example of the command output as it appears in CloudWatch Logs\.

```
Group - /aws/ssm/AWS-RunShellScript
Streams – 
1234-567-8910/i-abcd-efg-hijk/AWS-RunPowerShellScript/stdout
24/1234-567-8910/i-abcd-efg-hijk/AWS-RunPowerShellScript/stderr
```