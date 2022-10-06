# Configuring Amazon CloudWatch Logs for Run Command<a name="sysman-rc-setting-up-cwlogs"></a>

When you send a command by using Run Command, a capability of AWS Systems Manager, you can specify where you want to send the command output\. By default, Systems Manager returns only the first 48,000 characters of the command output\. If you want to view the full details of the command output, you can specify an Amazon Simple Storage Service \(Amazon S3\) bucket\. Or you can specify Amazon CloudWatch Logs\. If you specify CloudWatch Logs, Run Command periodically sends all command output and error logs to CloudWatch Logs\. You can monitor output logs in near real\-time, search for specific phrases, values, or patterns, and create alarms based on the search\. 

If you configured your node or on\-premises hybrid machine to use the AWS Identity and Access Management \(IAM\) managed policies `AmazonSSMManagedInstanceCore` and `CloudWatchAgentServerPolicy`, then your node requires no additional configuration to send output to CloudWatch Logs\. Choose this option if sending commands from the console, or add the `cloud-watch-output-config` section and `CloudWatchOutputEnabled` parameter if using the AWS Command Line Interface \(AWS CLI\), AWS Tools for Windows PowerShell, or an API operation\. The `cloud-watch-output-config` section and `CloudWatchOutputEnabled` parameter are described in more detail later in this topic\.

For information about adding policies to an instance profile for EC2 instances, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. For information about adding policies to a service role for on\-premises servers and virtual machines that you plan to use as managed nodes, see [Create an IAM service role for a hybrid environment](sysman-service-role.md)\.

For information about updating an existing instance profile, see [Add permissions to a Systems Manager instance profile \(console\)](setup-instance-profile.md#instance-profile-add-permissions)\.

If you're using a custom policy on your nodes, update the policy on each node to allow Systems Manager to send output and logs to CloudWatch Logs\. Add the following policy objects to your custom policy\. For more information about updating an IAM policy, see [Editing IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-edit.html) in the *IAM User Guide*\.

```
{
    "Effect": "Allow",
    "Action": "logs:DescribeLogGroups",
    "Resource": "*"
},
{
   "Effect":"Allow",
   "Action":[
      "logs:CreateLogGroup",
      "logs:CreateLogStream",
      "logs:DescribeLogStreams",
      "logs:PutLogEvents"
   ],
   "Resource":"arn:aws:logs:*:*:log-group:/aws/ssm/*"
},
```

## Specifying CloudWatch Logs when you send commands<a name="sysman-rc-setting-up-cwlogs-send"></a>

To specify CloudWatch Logs as the output when you send a command from the AWS Management Console, choose **CloudWatch Output** in the **Output options** section\. Optionally, you can specify the name of CloudWatch Logs group where you want to send command output\. If you don't specify a group name, Systems Manager automatically creates a log group for you\. The log group uses the following naming format: `/aws/ssm/SystemsManagerDocumentName`

If you run commands by using the AWS CLI, specify the `cloud-watch-output-config` section in your command\. This section allows you to specify the `CloudWatchOutputEnabled` parameter, and optionally, the `CloudWatchLogGroupName` parameter\. Here is an example\.

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --instance-ids "<Instance-ID>" \
    --document-name "AWS-RunShellScript" \
    --parameters "commands=echo helloWorld" \
    --cloud-watch-output-config "CloudWatchOutputEnabled=true,CloudWatchLogGroupName=<LogGroupName>"
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name "AWS-RunPowerShellScript" ^
    --parameters commands=["echo helloWorld"] ^
    --targets "Key=instanceids,Values=an instance ID" ^
    --cloud-watch-output-config '{"CloudWatchLogGroupName":"log group name","CloudWatchOutputEnabled":true}'
```

------

## Viewing command output in CloudWatch Logs<a name="sysman-rc-setting-up-cwlogs-view"></a>

As soon as the command starts to run, Systems Manager sends output to CloudWatch Logs in near\-real time\. The output in CloudWatch Logs uses the following format:

`CommandID/InstanceID/PluginID/stdout` 

`CommandID/InstanceID/PluginID/stderr`

Output from the execution is uploaded every 30 seconds or when the buffer exceeds 200 KB, whichever happens first\.

**Note**  
Log streams are only created when output data is available\. For example, if there is no error data for an execution, the stderr stream isn't created\.

Here is an example of the command output as it is displayed in CloudWatch Logs\.

```
Group - /aws/ssm/AWS-RunShellScript
Streams – 
1234-567-8910/i-abcd-efg-hijk/AWS-RunPowerShellScript/stdout
24/1234-567-8910/i-abcd-efg-hijk/AWS-RunPowerShellScript/stderr
```
