# Use a maintenance window to send a command that returns status notifications<a name="monitoring-sns-mw-register"></a>

The following procedures show how to register an AWS Systems Manager Run Command task with your maintenance window using the Systems Manager console or the AWS Command Line Interface \(AWS CLI\)\. The procedures also describe how to configure the Run Command task to return status notifications\.

**Before You Begin**  
If you haven't created a maintenance window or registered targets, see [Working with maintenance windows \(console\)](sysman-maintenance-working.md) for steps on how to create a maintenance window and register targets\.

To receive notifications from the Amazon SNS service, you must attach an `iam:PassRole` policy to the Maintenance Windows service role specified in the registered task\. If you haven't added` iam:PassRole` permissions to your Maintenance Windows service role, see [Task 5: Attach the iam:PassRole policy to your maintenance window role](monitoring-sns-notifications.md#monitoring-sns-passpolicy-mw)\. 

## Registering a Run Command task to a maintenance window that returns notifications \(console\)<a name="monitoring-sns-mw-register-console"></a>

Use the following procedure to register a Run Command task that is configured to return status notifications to your maintenance window using the Systems Manager console\.

**To register a Run Command task with your maintenance window that returns notifications \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Maintenance Windows**\.

1. Select the maintenance window for which you would like to register a Run Command task configured to send Amazon SNS notifications\.

1. Choose **Actions** and then choose **Register Run Command task**\.

1. In the **Name** field, enter a name for the task\.

1. In the **Description** field, enter a description\.

1. From the **Document** list, choose a Command document\.

1. In the **Task priority** list, specify a priority for this task\. 1 is the highest priority\. Tasks in a maintenance window are scheduled in priority order\. Tasks that have the same priority are scheduled in parallel\.

1. In the **Targets** section, identify the instances on which you want to run this operation by specifying tags, selecting instances manually, or specifying a resource group\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Some of my instances are missing](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. \(Optional\) For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed instances or by specifying AWS resource groups, and you are not certain how many instances are targeted, then restrict the number of instances that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Instances still processing the command might also send errors\.

1. In the ** IAM service role** area, choose the Maintenance Windows service role that has `iam:PassRole` permissions to the SNS role\.
**Note**  
You must add `iam:PassRole` permissions to the Maintenance Windows role to enable Systems Manager to pass the SNS role to Amazon SNS\. If you haven't added `iam:PassRole` permissions, see Task 5 in the topic [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an S3 bucket** box\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. In addition, if the specified S3 bucket is in a different AWS account, ensure that the instance profile associated with the instance has the necessary permissions to write to that bucket\.

1. In the **SNS notifications** section, choose **Enable SNS Notifications**\.

1. In the **IAM role** section, choose the SNS role to trigger Amazon SNS notifications\.

1. In the **SNS topic** section, type or paste the Amazon SNS topic ARN to be used\.

1. In the **Event type** section, choose the events for which you want to receive notifications\.

1. In the **Notification type** section, choose to receive notifications for each copy of a command sent to multiple instances \(invocations\) or the command summary\.

1. In the **Input Parameters** section, enter the required parameters based on the Command document you chose\.

1. Choose **Register Run Command task**\.

1. After the next execution of your maintenance window, check your email for a message from Amazon SNS and open the email\. Amazon SNS can take a few minutes to send the mail\.

## Registering a Run Command task to a maintenance window that returns notifications \(CLI\)<a name="monitoring-sns-mw-register-cli"></a>

Use the following procedure to register a Run Command task that is configured to return status notifications to your maintenance window using the AWS CLI\.

**To register a Run Command task with your maintenance window that returns notifications \(CLI\)**
**Note**  
To better manage your task options, this procedure uses the command option `--cli-input-json`, with option values stored in a JSON file\.

1. On your local machine, create a file named `RunCommandTask.json`\.

1. Paste the following contents into the file:

   ```
   {
       "Name": "Name",
       "Description": "Description",
       "WindowId": "mw-0c50858d01EXAMPLE",
       "ServiceRoleArn": "arn:aws:iam::111122223333:role/MaintenanceWindowIAMRole",
       "MaxConcurrency": "1",
       "MaxErrors": "1",
       "Priority": 3,
       "Targets": [
           {
               "Key": "WindowTargetIds",
               "Values": [
                   "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
               ]
           }
       ],
       "TaskType": "RUN_COMMAND",
       "TaskArn": "CommandDocumentName",
       "TaskInvocationParameters": {
           "RunCommand": {
               "Comment": "Comment",
               "TimeoutSeconds": 3600,
               "NotificationConfig": {
                   "NotificationArn": "arn:aws:sns:region:123456789012:SNSTopicName",
                   "NotificationEvents": [
                       "All"
                   ],
                   "NotificationType": "Command"
               },
               "ServiceRoleArn": "arn:aws:iam::123456789012:role/SNSIAMRole"
           }
       }
   }
   ```

1. Replace the example values with information about your own resources\. 

   You can also restore options we have omitted from this example if you want to use them\. For example, you can save command output to an S3 bucket\. 

   For more information, see For more information, see [register\-task\-with\-maintenance\-window](https://docs.aws.amazon.com/cli/latest/reference/ssm/register-task-with-maintenance-window.html)\.

1. Save the file\.

1. In the directory on your local machine where you saved the file, run the following command:

   `aws ssm register-task-with-maintenance-window --cli-input-json file://RunCommandTask.json`
**Important**  
Be sure to include `file://` before the file name\. It is required in this command\.

   If successful, the command returns information similar to the following:

   ```
   {
       "WindowTaskId": "j2l8d5b5c-mw66-tk4d-r3g9-1d4d1EXAMPLE"
   }
   ```

1. After the next execution of your maintenance window, check your email for a message from Amazon SNS and open the email\. Amazon SNS can take a few minutes to send the mail\.

For more information about registering tasks for maintenance window from the command line, see [Amazon EC2 Systems Manager API Reference](https://docs.aws.amazon.com/ssm/latest/APIReference/) and the [Systems Manager AWS CLI Reference](https://docs.aws.amazon.com/cli/latest/reference/ssm/index.html)\.