# Auditing and Logging Session Activity<a name="session-manager-logging-auditing"></a>

In addition to providing information about current and completed sessions in the Systems Manager console, Session Manager provides you with options for auditing and logging session activity in your AWS account\. This allows you to do the following:
+ Create and store session logs for archival purposes\.
+ Generate a report showing details of every connection made to your instances using Session Manager over the past 30 days\.
+ Generate notifications of session activity in your AWS account, such as Amazon Simple Notification Service \(Amazon SNS\) notifications\.
+ Automatically initiate another action on an AWS resource as the result of session activity, such as running an AWS Lambda function, starting an AWS CodePipeline pipeline, or running an AWS Systems Manager Run Command document\.

**Note**  
If you are using Windows Server 2012 or earlier, the data in your logs might not be formatted optimally\. We recommend using Windows Server 2012 R2 and later for optimal log formats\.  
If you are using Linux instances, ensure that the screen utility is installed\. If it is not, your log data might be truncated\. On Amazon Linux, Amazon Linux 2, and Ubuntu Server, the screen utility is installed by default\. To install screen manually, depending on your version of Linux, run either `sudo yum install screen` or `sudo apt-get install screen`\.

Refer to the following topics for more information about auditing and logging options for Session Manager\.

## Audit Session Activity Using AWS CloudTrail<a name="session-manager-logging-auditing-cloudtrail"></a>

AWS CloudTrail captures session API calls through the Systems Manager console, the AWS CLI, and the Systems Manager SDK\. The information can be viewed on the CloudTrail console or stored in a specified Amazon S3 bucket\. One bucket is used for all CloudTrail logs for your account\. 

For more information, see [Log AWS Systems Manager API Calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)\. 

## Log Session Data Using Amazon S3<a name="session-manager-logging-auditing-s3"></a>

You can choose to store session log data in a specified Amazon S3 bucket for auditing purposes\. By default, logs are sent to an encrypted S3 bucket\. Encryption is performed using your choice of an AWS Key Management Service \(AWS KMS\) key or an Amazon S3 Server\-Side Encryption \(SSE\) key \(AES\-256\)\. 

**S3 Bucket Encryption**  
In order to send logs to your S3 bucket with encryption, encryption must be enabled on the bucket\. For more information about S3 bucket encryption, see [Amazon S3 Default Encryption for S3 Buckets](Amazon Simple Storage Service Developer Guidebucket-encryption.html)\.

**Customer\-managed CMK**  
If you are using a KMS customer master key \(CMK\) that you manage yourself \(a customer managed CMK\) to encrypt your bucket, then the IAM instance profile attached to your instances must have explicit permissions to read the CMK\. If you use an AWS managed CMK, the instance does not require this explicit permission\. For more information about providing the instance profile with access to use the CMK, see [Allows Key Users to Use the CMK](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-default-allow-users) in the *AWS Key Management Service Developer Guide*\.

Follow these steps to configure Session Manager to store session logs in an Amazon S3 bucket\.

**Note**  
You can also use the AWS CLI to specify or change the S3 bucket that session data is sent to\. For information, see [Use the AWS CLI to Update Session Manager Preferences](getting-started-configure-preferences-cli.md)\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Select the check box next to **S3 bucket**\.

1. \(Optional\) If you do not want to encrypt the log data that is sent to the S3 bucket, select the check box next to **Encrypt log data**\. Otherwise, log data is encrypted using your AWS Key Management Service \(AWS KMS\) key\.

1. For **S3 bucket name**, select one of the following:
   + **Choose a bucket name from the list**: Use a bucket that has already been created in your account to store session log data\.
   + **Enter a bucket name in the text box**: Create a new S3 bucket to store session logs\.

1. \(Optional\) For **S3 key prefix**, enter the name of an existing or new folder to store logs in the selected bucket\.

1. Choose **Save**\.

For more information about working with Amazon S3 and S3 buckets, see the *[Amazon Simple Storage Service Getting Started Guide](https://docs.aws.amazon.com/AmazonS3/latest/gsg/)* and the *[Amazon Simple Storage Service Console User Guide](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/)*\.

## Log Session Data Using Amazon CloudWatch Logs<a name="session-manager-logging-auditing-cloudwatch-logs"></a>

Amazon CloudWatch Logs lets you monitor, store, and access log files from various AWS services\. You can stream session log data to a CloudWatch Logs log group for auditing purposes\. Log data can be streamed to your log group with or without encryption using your AWS KMS key\. 

Follow these steps to configure Session Manager to stream session log data to a CloudWatch Logs log group\.

**Note**  
You can also use the AWS CLI to specify or change the CloudWatch log group that session data is sent to\. For information, see [Use the AWS CLI to Update Session Manager Preferences](getting-started-configure-preferences-cli.md)\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Select the check box next to **CloudWatch logs**\.

1. \(Optional\) If you do not want to encrypt the log data that is sent to CloudWatch Logs, select the check box next to **Encrypt log data**\. Otherwise, log data is encrypted using your AWS Key Management Service \(AWS KMS\) key\.

1. For **CloudWatch logs**, to specify the CloudWatch log group in your AWS account to upload session logs to, select one of the following:
   + **Choose a log group from the list**: Use a log group that has already been created in your account to store session log data\.
   + **Enter a log group name in the text box**: Create a new log group to store session logs\.

1. Choose **Save**\.

For more information about working with CloudWatch Logs, see the *[Amazon CloudWatch Logs User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)*\.

## Monitor Session Activity Using Amazon CloudWatch Events<a name="session-manager-logging-auditing-cloudwatch-events"></a>

CloudWatch Events lets you set up rules to detect when changes happen to AWS resources\. You can create a rule to detect when a user in your organization starts or terminates a session, and then, for example, receive a notification through Amazon SNS about the event\. 

CloudWatch Events support for Session Manager relies on records of API actions that were recorded by CloudTrail\. \(You can use CloudTrail integration with CloudWatch Events to respond to most AWS Systems Manager events\.\)

The following steps outline how to trigger notifications through Amazon Simple Notification Service \(Amazon SNS\) when a Session Manager API event occurs, such as StartSession\.

1. Create an Amazon SNS topic to use for sending notifications when the Session Manager event occurs that you want to track\.

   For more information, see [Create a Topic](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html) in the *Amazon Simple Notification Service Developer Guide*\.

1. Create a CloudWatch Events rule to invoke the Amazon SNS target for the type of Session Manager event you want to track\.

   For information about how to create the rule, see [Creating a CloudWatch Events Rule That Triggers on an Event](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/Create-CloudWatch-Events-Rule.html) in the *Amazon CloudWatch Events User Guide*\.

   As you follow the steps to create the rule, make the following selections:
   + For **Service Name**, choose **EC2 Simple Systems Manager \(SSM\)**\.
   + For **Event Type**, choose **AWS API Call via CloudTrail**\.
   + Choose **Specific operation\(s\)**, and then enter the Session Manager command or commands \(one at a time\) you want to receive notifications for\. You can choose StartSession, ResumeSession, and TerminateSession\. \(CloudWatch Events doesn't support `Get*`,` List*`, and `Describe*` commands\.\)
   + In the **Targets** list, choose **SNS topic**\. In the **Topic** list, choose the name of the Amazon SNS topic you created in Step 1\.

For more information, see the *[Amazon CloudWatch Events User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/)* and the *[Amazon Simple Notification Service Getting Started Guide](https://docs.aws.amazon.com/sns/latest/gsg/)*\.