# Use Run Command to send a command that returns status notifications<a name="monitoring-sns-rc-send"></a>

The following procedures show how to use the AWS Command Line Interface \(AWS CLI\) or AWS Systems Manager console to send a command through Run Command, a capability of AWS Systems Manager, that is configured to return status notifications\.

## Sending a Run Command that returns notifications \(console\)<a name="monitoring-sns-rc-send-console"></a>

Use the following procedure to send a command through Run Command that is configured to return status notifications using the Systems Manager console\.

**To send a command that returns notifications \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run command**\.

1. In the **Command document** list, choose a Systems Manager document\.

1. In the **Command parameters** section, specify values for required parameters\.

1. In the **Targets** section, choose the managed nodes on which you want to run this operation by specifying tags, selecting instances or edge devices manually, or specifying a resource group\.
**Note**  
If a managed node you expect to see isn't listed, see [Troubleshooting managed node availability](troubleshooting-managed-instances.md) for troubleshooting tips\.

1. For **Other parameters**:
   + For **Comment**, enter information about this command\.
   + For **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of managed nodes on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed nodes or by specifying AWS resource groups, and you aren't certain how many managed nodes are targeted, then restrict the number of targets that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other managed nodes after it fails on either a number or a percentage of nodes\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Managed nodes still processing the command might also send errors\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an S3 bucket** box\. Enter the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile \(for EC2 instances\) or IAM service role \(on\-premises machines\) assigned to the instance, nor those of the user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md) or [Create an IAM service role for a hybrid environment](sysman-service-role.md)\. In addition, if the specified S3 bucket is in a different AWS account, make sure that the instance profile or IAM service role associated with the managed node has the necessary permissions to write to that bucket\.

1. In the **SNS Notifications** section, choose **Enable SNS notifications**\.

1. For **IAM role**, choose the Amazon SNS IAM role ARN you created in Task 3 in [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

1. For **SNS topic**, enter the Amazon SNS topic ARN to be used\.

1. For **Event notifications**, choose the events for which you want to receive notifications\.

1. For **Change notifications**, choose to receive notifications for the command summary only \(**Command status changes**\) or for each copy of a command sent to multiple nodes \(**Command status on each instance changes**\) \.

1. Choose **Run**\.

1. Check your email for a message from Amazon SNS and open the email message\. Amazon SNS can take several minutes to send the email message\.

## Sending a Run Command that returns notifications \(CLI\)<a name="monitoring-sns-rc-send-cli"></a>

Use the following procedure to send a command through Run Command that is configured to return status notifications using the AWS CLI\.

**To send a command that returns notifications \(CLI\)**

1. Open the AWS CLI\.

1. Specify parameters in the following command to target based on managed node IDs\.

   ```
   aws ssm send-command --instance-ids "ID-1, ID-2" --document-name "Name" --parameters '{"commands":["input"]}' --service-role "SNSRoleARN" --notification-config '{"NotificationArn":"SNSTopicName","NotificationEvents":["All"],"NotificationType":"Command"}'
   ```

   Following is an example\.

   ```
   aws ssm send-command --instance-ids "i-02573cafcfEXAMPLE, i-0471e04240EXAMPLE" --document-name "AWS-RunPowerShellScript" --parameters '{"commands":["Get-Process"]}' --service-role "arn:aws:iam::111122223333:role/SNS_Role" --notification-config '{"NotificationArn":"arn:aws:sns:us-east-1:111122223333:SNSTopic","NotificationEvents":["All"],"NotificationType":"Command"}'
   ```

**Alternative commands**  
Specify parameters in the following command to target managed instances using tags\.

   ```
   aws ssm send-command --targets "Key=tag:TagName,Values=TagKey" --document-name "Name" --parameters '{"commands":["input"]}' --service-role "SNSRoleARN" --notification-config '{"NotificationArn":"SNSTopicName","NotificationEvents":["All"],"NotificationType":"Command"}'
   ```

   Following is an example\.

   ```
   aws ssm send-command --targets "Key=tag:Environment,Values=Dev" --document-name "AWS-RunPowerShellScript" --parameters '{"commands":["Get-Process"]}' --service-role "arn:aws:iam::111122223333:role/SNS_Role" --notification-config '{"NotificationArn":"arn:aws:sns:us-east-1:111122223333:SNSTopic","NotificationEvents":["All"],"NotificationType":"Command"}'
   ```

1. Press **Enter**\.

1. Check your email for a message from Amazon SNS and open the email message\. Amazon SNS can take several minutes to send the email message\.

For more information, see [send\-command](https://docs.aws.amazon.com/cli/latest/reference/ssm/send-command.html) in the *AWS CLI Command Reference*\.