# Logging session activity<a name="session-manager-logging"></a>

In addition to providing information about current and completed sessions in the Systems Manager console, Session Manager provides you with options for logging session activity in your AWS account\. This allows you to do the following:
+ Create and store session logs for archival purposes\.
+ Generate a report showing details of every connection made to your instances using Session Manager over the past 30 days\.
+ Generate notifications of session activity in your AWS account, such as Amazon Simple Notification Service \(Amazon SNS\) notifications\.
+ Automatically initiate another action on an AWS resource as the result of session activity, such as running an AWS Lambda function, starting an AWS CodePipeline pipeline, or running an AWS Systems Manager Run Command document\.

**Important**  
Take note of the following requirements and limitations for Session Manager:  
Session Manager logs the commands you enter and their output during a session depending on your session preferences\. To prevent sensitive data, such as passwords, from being viewed in your session logs we recommend using the following commands when entering sensitive data during a session\.  

  ```
  stty -echo; read passwd; stty echo;
  ```

  ```
  $Passwd = Read-Host -AsSecureString
  ```
If you are using Windows Server 2012 or earlier, the data in your logs might not be formatted optimally\. We recommend using Windows Server 2012 R2 and later for optimal log formats\.
If you are using Linux or macOS instances, ensure that the screen utility is installed\. If it is not, your log data might be truncated\. On Amazon Linux, Amazon Linux 2, and Ubuntu Server, the screen utility is installed by default\. To install screen manually, depending on your version of Linux, run either `sudo yum install screen` or `sudo apt-get install screen`\.
Logging is not available for Session Manager sessions that connect through port forwarding or SSH\. This is because SSH encrypts all session data, and Session Manager only serves as a tunnel for SSH connections\.

For more information about the permissions required to use Amazon S3 or Amazon CloudWatch Logs for logging session data, see [Creating an instance profile with permissions for Session Manager and Amazon S3 and CloudWatch Logs \(console\)](getting-started-create-iam-instance-profile.md#create-iam-instance-profile-ssn-logging)\.

Refer to the following topics for more information about logging options for Session Manager\.

**Topics**
+ [Streaming session data using Amazon CloudWatch Logs \(console\)](#session-manager-logging-cwl-streaming)
+ [Logging session data using Amazon S3 \(console\)](#session-manager-logging-s3)
+ [Logging session data using Amazon CloudWatch Logs \(console\)](#session-manager-logging-cloudwatch-logs)

## Streaming session data using Amazon CloudWatch Logs \(console\)<a name="session-manager-logging-cwl-streaming"></a>

You can send a continuous stream of session data logs to Amazon CloudWatch Logs\. Essential details, like the commands a user has run in a session, the ID of the user who ran the commands, and timestamps for when the session data is streamed to CloudWatch Logs, are included when streaming session data\. When streaming session data, the logs are JSON\-formatted to help you integrate with your existing logging solutions\. Streaming session data is not supported for interactive commands\.

**Note**  
To stream session data from Windows Server instances, you must have PowerShell 5\.1 or later installed\. By default, Windows Server 2016 and later have the required PowerShell version installed\. However, Windows Server 2012 and 2012 R2 do not have the required PowerShell version installed by default\. If you have not already updated PowerShell on your Windows Server 2012 or 2012 R2 instances, you can do so using Run Command\. For information on updating PowerShell using Run Command, see [Update PowerShell using Run Command](rc-console.md#rc-console-pwshexample)\.

**Important**  
If you have the **PowerShell Transcription** policy setting configured on your Windows Server instances, you will not be able to stream session data\.

**To stream session data using Amazon CloudWatch Logs \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Select the check box next to **Enable** under **CloudWatch logging**\.

1. Choose the **Stream session logs** option\.

1. \(Recommended\) Select the check box next to **Allow only encrypted CloudWatch log groups**\. With this option enabled, log data is encrypted using the server\-side encryption key specified for the log group\. If you do not want to encrypt the log data that is sent to CloudWatch Logs, clear the check box\. You must also clear the check box if encryption is not enabled on the log group\.

1. For **CloudWatch logs**, to specify the existing CloudWatch Logs log group in your AWS account to upload session logs to, select one of the following:
   + Enter the name of a log group in the text box that has already been created in your account to store session log data\.
   + **Browse log groups**: Select a log group that has already been created in your account to store session log data\.

1. Choose **Save**\.

## Logging session data using Amazon S3 \(console\)<a name="session-manager-logging-s3"></a>

You can choose to store session log data in a specified Amazon Simple Storage Service \(Amazon S3\) bucket for debugging and troubleshooting purposes\. The default option is for logs to be sent to an encrypted Amazon S3 bucket\. Encryption is performed using the key specified for the bucket, either an AWS KMS key or an Amazon S3 Server\-Side Encryption \(SSE\) key \(AES\-256\)\. 

**Important**  
When you use virtual hosted–style buckets with Secure Sockets Layer \(SSL\), the SSL wildcard certificate only matches buckets that don't contain periods\. To work around this, use HTTP or write your own certificate verification logic\. We recommend that you do not use periods \("\."\) in bucket names when using virtual hosted–style buckets\.

**Amazon S3 bucket encryption**  
In order to send logs to your Amazon S3 bucket with encryption, encryption must be enabled on the bucket\. For more information about Amazon S3 bucket encryption, see [Amazon S3 Default Encryption for S3 Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/bucket-encryption.html)\.

**Customer\-managed key**  
If you are using a KMS key that you manage yourself to encrypt your bucket, then the IAM instance profile attached to your instances must have explicit permissions to read the key\. If you use an AWS managed key, the instance does not require this explicit permission\. For more information about providing the instance profile with access to use the key, see [Allows Key Users to Use the key](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html#key-policy-default-allow-users) in the *AWS Key Management Service Developer Guide*\.

Follow these steps to configure Session Manager to store session logs in an Amazon S3 bucket\.

**Note**  
You can also use the AWS CLI to specify or change the Amazon S3 bucket that session data is sent to\. For information, see [Update Session Manager preferences \(command line\)](getting-started-configure-preferences-cli.md)\.

**To log session data using Amazon S3 \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Select the check box next to **Enable** under **S3 logging**\.

1. \(Recommended\) Select the check box next to **Allow only encrypted S3 buckets**\. With this option enabled, log data is encrypted using the server\-side encryption key specified for the bucket\. If you do not want to encrypt the log data that is sent to Amazon S3, clear the check box\. You must also clear the check box if encryption is not enabled on the S3 bucket\.

1. For **S3 bucket name**, select one of the following:
**Note**  
We recommend that you do not use periods \("\."\) in bucket names when using virtual hosted–style buckets\. For more information about Amazon S3 bucket\-naming conventions, see [Bucket Restrictions and Limitations](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html#bucketnamingrules) in the *Amazon Simple Storage Service Developer Guide*\.
   + **Choose a bucket name from the list**: Select an Amazon S3 bucket that has already been created in your account to store session log data\.
   + **Enter a bucket name in the text box**: Enter the name of an Amazon S3 bucket that has already been created in your account to store session log data\.

1. \(Optional\) For **S3 key prefix**, enter the name of an existing or new folder to store logs in the selected bucket\.

1. Choose **Save**\.

For more information about working with Amazon S3 and Amazon S3 buckets, see the *[Amazon Simple Storage Service Getting Started Guide](https://docs.aws.amazon.com/AmazonS3/latest/gsg/)* and the *[Amazon Simple Storage Service Console User Guide](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/)*\.

## Logging session data using Amazon CloudWatch Logs \(console\)<a name="session-manager-logging-cloudwatch-logs"></a>

With Amazon CloudWatch Logs, you can monitor, store, and access log files from various AWS services\. You can send session log data to a CloudWatch Logs log group for debugging and troubleshooting purposes\. The default option is for log data to be sent with encryption using your KMS key, but you can send the data to your log group with or without encryption\. 

Follow these steps to configure AWS Systems Manager Session Manager to send session log data to a CloudWatch Logs log group at the end of your sessions\.

**Note**  
You can also use the AWS CLI to specify or change the CloudWatch Logs log group that session data is sent to\. For information, see [Update Session Manager preferences \(command line\)](getting-started-configure-preferences-cli.md)\.

**To log session data using Amazon CloudWatch Logs \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Select the check box next to **Enable** under **CloudWatch logging**\.

1. Choose the **Upload session logs** option\.

1. \(Recommended\) Select the check box next to **Allow only encrypted CloudWatch log groups**\. With this option enabled, log data is encrypted using the server\-side encryption key specified for the log group\. If you do not want to encrypt the log data that is sent to CloudWatch Logs, clear the check box\. You must also clear the check box if encryption is not enabled on the log group\.

1. For **CloudWatch logs**, to specify the existing CloudWatch Logs log group in your AWS account to upload session logs to, select one of the following:
   + **Choose a log group from the list**: Select a log group that has already been created in your account to store session log data\.
   + **Enter a log group name in the text box**: Enter the name of a log group that has already been created in your account to store session log data\.

1. Choose **Save**\.

For more information about working with CloudWatch Logs, see the *[Amazon CloudWatch Logs User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)*\.