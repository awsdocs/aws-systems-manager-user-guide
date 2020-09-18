# Auditing and logging session activity<a name="session-manager-logging-auditing"></a>

In addition to providing information about current and completed sessions in the Systems Manager console, Session Manager provides you with options for auditing and logging session activity in your AWS account\. This allows you to do the following:
+ Create and store session logs for archival purposes\.
+ Generate a report showing details of every connection made to your instances using Session Manager over the past 30 days\.
+ Generate notifications of session activity in your AWS account, such as Amazon Simple Notification Service \(Amazon SNS\) notifications\.
+ Automatically initiate another action on an AWS resource as the result of session activity, such as running an AWS Lambda function, starting an AWS CodePipeline pipeline, or running an AWS Systems Manager Run Command document\.

**Important**  
Take note of the following requirements and limitations for Session Manager\.  
If you are using Windows Server 2012 or earlier, the data in your logs might not be formatted optimally\. We recommend using Windows Server 2012 R2 and later for optimal log formats\.
If you are using Linux instances, ensure that the screen utility is installed\. If it is not, your log data might be truncated\. On Amazon Linux, Amazon Linux 2, and Ubuntu Server, the screen utility is installed by default\. To install screen manually, depending on your version of Linux, run either `sudo yum install screen` or `sudo apt-get install screen`\.
Logging and auditing are not available for Session Manager sessions that connect through port forwarding or SSH\. This is because SSH encrypts all session data, and Session Manager only serves as a tunnel for SSH connections\.

For more information about the permissions required to use Amazon S3 or Amazon CloudWatch Logs for logging session data, see [Creating an instance profile with permissions for Session Manager and Amazon S3 and CloudWatch Logs \(console\)](getting-started-create-iam-instance-profile.md#create-iam-instance-profile-ssn-logging)\.

Refer to the following topics for more information about auditing and logging options for Session Manager\.

## Audit session activity using AWS CloudTrail<a name="session-manager-logging-auditing-cloudtrail"></a>

AWS CloudTrail captures session API calls through the Systems Manager console, the AWS CLI, and the Systems Manager SDK\. The information can be viewed on the CloudTrail console or stored in a specified Amazon S3 bucket\. One bucket is used for all CloudTrail logs for your account\. 

For more information, see [Logging AWS Systems Manager API calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)\. 

## Logging session data using Amazon S3 \(console\)<a name="session-manager-logging-auditing-s3"></a>

You can choose to store session log data in a specified S3 bucket for auditing purposes\. The default option is for logs to be sent to an encrypted S3 bucket\. Encryption is performed using the key specified for the bucket, either an AWS Key Management Service \(AWS KMS\) key or an Amazon S3 Server\-Side Encryption \(SSE\) key \(AES\-256\)\. 

**Important**  
When you use virtual hosted–style buckets with Secure Sockets Layer \(SSL\), the SSL wildcard certificate only matches buckets that don't contain periods\. To work around this, use HTTP or write your own certificate verification logic\. We recommend that you do not use periods \("\."\) in bucket names when using virtual hosted–style buckets\.

**S3 Bucket Encryption**  
In order to send logs to your S3 bucket with encryption, encryption must be enabled on the bucket\. For more information about S3 bucket encryption, see [Amazon S3 Default Encryption for S3 Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/bucket-encryption.html)\.

**Customer\-managed CMK**  
If you are using an AWS KMS customer master key \(CMK\) that you manage yourself \(a customer\-managed CMK\) to encrypt your bucket, then the IAM instance profile attached to your instances must have explicit permissions to read the CMK\. If you use an AWS\-managed CMK, the instance does not require this explicit permission\. For more information about providing the instance profile with access to use the CMK, see [Allows Key Users to Use the CMK](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-default-allow-users) in the *AWS Key Management Service Developer Guide*\.

Follow these steps to configure Session Manager to store session logs in an Amazon S3 bucket\.

**Note**  
You can also use the AWS CLI to specify or change the S3 bucket that session data is sent to\. For information, see [Update Session Manager preferences \(command line\)](getting-started-configure-preferences-cli.md)\.

**To log session data using Amazon S3 \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Select the check box next to **S3 bucket**\.

1. \(Optional\) If you do not want to encrypt the log data that is sent to the S3 bucket, clear the check box next to **Encrypt log data**\. Otherwise, log data is encrypted using the server\-side encryption key specified for the bucket\. You must also clear the check box if encryption is not enabled on the bucket\.

1. For **S3 bucket name**, select one of the following:
**Note**  
We recommend that you do not use periods \("\."\) in bucket names when using virtual hosted–style buckets\. For more information about S3 bucket\-naming conventions, see [Bucket Restrictions and Limitations](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html#bucketnamingrules) in the *Amazon Simple Storage Service Developer Guide*\.
   + **Choose a bucket name from the list**: Select an S3 bucket that has already been created in your account to store session log data\.
   + **Enter a bucket name in the text box**: Enter the name of an S3 bucket that has already been created in your account to store session log data\.

1. \(Optional\) For **S3 key prefix**, enter the name of an existing or new folder to store logs in the selected bucket\.

1. Choose **Save**\.

For more information about working with Amazon S3 and S3 buckets, see the *[Amazon Simple Storage Service Getting Started Guide](https://docs.aws.amazon.com/AmazonS3/latest/gsg/)* and the *[Amazon Simple Storage Service Console User Guide](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/)*\.

## Logging session data using Amazon CloudWatch Logs \(console\)<a name="session-manager-logging-auditing-cloudwatch-logs"></a>

Amazon CloudWatch Logs lets you monitor, store, and access log files from various AWS services\. You can send session log data to a CloudWatch Logs log group for auditing purposes\. The default option is for log data to be sent with encryption using your AWS KMS key, but you can send the data to your log group with or without encryption\. 

Follow these steps to configure Session Manager to send session log data to a CloudWatch Logs log group\.

**Note**  
You can also use the AWS CLI to specify or change the CloudWatch Logs log group that session data is sent to\. For information, see [Update Session Manager preferences \(command line\)](getting-started-configure-preferences-cli.md)\.

**To log session data using Amazon CloudWatch Logs \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Select the check box next to **CloudWatch logs**\.

1. \(Optional\) If you do not want to encrypt the log data that is sent to CloudWatch Logs, clear the check box next to **Encrypt log data**\. Otherwise, log data is encrypted using the server\-side encryption key specified for the log group\. You must also clear the check box if encryption is not enabled on the log group\.

1. For **CloudWatch logs**, to specify the existing CloudWatch Logs log group in your AWS account to upload session logs to, select one of the following:
   + **Choose a log group from the list**: Select a log group that has already been created in your account to store session log data\.
   + **Enter a log group name in the text box**: Enter the name of a log group that has already been created in your account to store session log data\.

1. Choose **Save**\.

For more information about working with CloudWatch Logs, see the *[Amazon CloudWatch Logs User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)*\.

## Monitoring session activity using Amazon EventBridge \(console\)<a name="session-manager-logging-auditing-eventbridge-events"></a>

EventBridge lets you set up rules to detect when changes happen to AWS resources\. You can create a rule to detect when a user in your organization starts or ends a session, and then, for example, receive a notification through Amazon SNS about the event\. 

EventBridge support for Session Manager relies on records of API actions that were recorded by CloudTrail\. \(You can use CloudTrail integration with EventBridge to respond to most AWS Systems Manager events\.\)

The following steps outline how to trigger notifications through Amazon Simple Notification Service \(Amazon SNS\) when a Session Manager API event occurs, such as StartSession\.

**To monitor session activity using Amazon EventBridge \(console\)**

1. Create an Amazon SNS topic to use for sending notifications when the Session Manager event occurs that you want to track\.

   For more information, see [Create a Topic](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html) in the *Amazon Simple Notification Service Developer Guide*\.

1. Create an EventBridge rule to invoke the Amazon SNS target for the type of Session Manager event you want to track\.

   For information about how to create the rule, see [Creating an EventBridge Rule That Triggers on an Event from an AWS Resource](https://docs.aws.amazon.com/eventbridge/latest/userguide/create-eventbridge-rule.html) in the *Amazon EventBridge User Guide*\.

   As you follow the steps to create the rule, make the following selections:
   + For **Service Name**, choose **EC2 Simple Systems Manager \(SSM\)**\.
   + For **Event Type**, choose **AWS API Call via CloudTrail**\.
   + Choose **Specific operation\(s\)**, and then enter the Session Manager command or commands \(one at a time\) you want to receive notifications for\. You can choose StartSession, ResumeSession, and TerminateSession\. \(EventBridge doesn't support `Get*`,` List*`, and `Describe*` commands\.\)
   + For**Targets**, choose **SNS topic**\. For **Topic**, choose the name of the Amazon SNS topic you created in Step 1\.

For more information, see the *[Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/)* and the *[Amazon Simple Notification Service Getting Started Guide](https://docs.aws.amazon.com/sns/latest/gsg/)*\.