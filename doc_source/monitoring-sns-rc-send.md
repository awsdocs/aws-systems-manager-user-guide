# Use Run Command to send a command that returns status notifications<a name="monitoring-sns-rc-send"></a>

The following procedures show how to use the AWS Command Line Interface \(AWS CLI\) or AWS Systems Manager console to send a Run Command that is configured to return status notifications\.

## Sending a Run Command that returns notifications \(console\)<a name="monitoring-sns-rc-send-console"></a>

Use the following procedure to send a command through Run Command that is configured to return status notifications using the Systems Manager console\.

**To send a command that returns notifications \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose a Systems Manager document\.

1. In the **Command parameters** section, specify values for required parameters\.

1. In the **Targets** section, identify the instances on which you want to run this operation by specifying tags, selecting instances manually, or specifying a resource group\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Some of my instances are missing](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. For **Other parameters**:
   + For **Comment**, type information about this command\.
   + For **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. \(Optional\) For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed instances or by specifying AWS resource groups, and you are not certain how many instances are targeted, then restrict the number of instances that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Instances still processing the command might also send errors\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an S3 bucket** box\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. In addition, if the specified S3 bucket is in a different AWS account, ensure that the instance profile associated with the instance has the necessary permissions to write to that bucket\.

1. In the **SNS Notifications** section, choose **Enable SNS notifications**\.

1. In the **IAM role** field, type or paste the SNS IAM role ARN you created in Task 3 in the topic [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

1. In the **SNS topic** field, type or paste the Amazon SNS topic ARN to be used\.

1. In the **Notify me on** field, choose the events for which you want to receive notifications\.

1. In the **Notify me for** field, choose to receive notifications for each copy of a command sent to multiple instances \(invocations\) or the command summary\.

1. Choose **Run**\.

1. Check your email for a message from Amazon SNS and open the email\. Amazon SNS can take a few minutes to send the mail\.

## Sending a Run Command that returns notifications \(CLI\)<a name="monitoring-sns-rc-send-cli"></a>

Use the following procedure to send a command through Run Command that is configured to return status notifications using the AWS CLI\.

**To send a command that returns notifications \(CLI\)**

1. Open the AWS CLI\.

1. Specify parameters in the following command to target based on managed instance IDs:

   ```
   aws ssm send-command --instance-ids "ID-1, ID-2" --document-name "Name" --parameters '{"commands":["input"]}' --service-role "SNSRoleARN" --notification-config '{"NotificationArn":"SNSTopicName","NotificationEvents":["All"],"NotificationType":"Command"}'
   ```

   For example:

   ```
   aws ssm send-command --instance-ids "i-02573cafcfEXAMPLE, i-0471e04240EXAMPLE" --document-name "AWS-RunPowerShellScript" --parameters '{"commands":["Get-Process"]}' --service-role "arn:aws:iam::111122223333:role/SNS_Role" --notification-config '{"NotificationArn":"arn:aws:sns:us-east-1:111122223333:SNSTopic","NotificationEvents":["All"],"NotificationType":"Command"}'
   ```

**Alternative commands**  
Specify parameters in the following command to target managed instances using tags:

   ```
   aws ssm send-command --targets "Key=tag:TagName,Values=TagKey" --document-name "Name" --parameters '{"commands":["input"]}' --service-role "SNSRoleARN" --notification-config '{"NotificationArn":"SNSTopicName","NotificationEvents":["All"],"NotificationType":"Command"}'
   ```

   For example:

   ```
   aws ssm send-command --targets "Key=tag:Environment,Values=Dev" --document-name "AWS-RunPowerShellScript" --parameters '{"commands":["Get-Process"]}' --service-role "arn:aws:iam::111122223333:role/SNS_Role" --notification-config '{"NotificationArn":"arn:aws:sns:us-east-1:111122223333:SNSTopic","NotificationEvents":["All"],"NotificationType":"Command"}'
   ```

1. Press Enter\.

1. Check your email for a message from Amazon SNS and open the email\. Amazon SNS can take a few minutes to send the mail\.

For more information about configuring Run Command from the command line, see [Amazon EC2 Systems Manager API Reference](https://docs.aws.amazon.com/ssm/latest/APIReference/) and the [Systems Manager AWS CLI Reference](https://docs.aws.amazon.com/cli/latest/reference/ssm/index.html)\.