# Log Systems Manager API Calls with AWS CloudTrail<a name="monitoring-cloudtrail-logs"></a>

AWS Systems Manager is integrated with CloudTrail, a service that captures API calls made by or on behalf of Systems Manager in your AWS account and delivers the log files to an Amazon S3 bucket you specify\. CloudTrail captures API calls from the Systems Manager console, from Systems Manager commands through the AWS CLI, from the AWS Tools for Windows PowerShell, or from Systems Manager APIs called directly\. Using the information collected by CloudTrail, you can determine which request was made to Systems Manager, the source IP address from which the request was made, who made the request, when it was made, and so on\. To learn more about CloudTrail, including how to configure and enable it, see the [AWS CloudTrail User Guide](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/)\.

**Topics**
+ [Systems Manager Information in CloudTrail](#monitoring-cloudtrail-logs-log-entries-about)
+ [Understanding Systems Manager Log File Entries](#monitoring-cloudtrail-logs-log-entries-example)

## Systems Manager Information in CloudTrail<a name="monitoring-cloudtrail-logs-log-entries-about"></a>

When CloudTrail logging is enabled in your AWS account, API calls made to Systems Manager are tracked in log files\. Systems Manager records are written together with other AWS service records in a log file\. CloudTrail determines when to create and write to a new file based on a time period and file size\. 

All of the Systems Manager actions are logged and documented in the [AWS Systems Manager API Reference](http://docs.aws.amazon.com/systems-manager/latest/APIReference/), the [AWS Systems Manager section of the AWS CLI Command Reference](http://docs.aws.amazon.com/cli/latest/reference/ssm/index.html), and the [AWS Systems Manager section of the AWS Tools for PowerShell Cmdlet Reference](http://docs.aws.amazon.com/powershell/latest/reference/items/Amazon_Simple_Systems_Management_cmdlets.html)\. For example, calls to create, delete, and edit Maintenance Windows and create custom patch baselines generate entries in CloudTrail log files\.

Every log entry contains information about who generated the request\. The user identity information in the log helps you determine whether the request was made with root or IAM user credentials, with temporary security credentials for a role or federated user, or by another AWS service\. For more information, see the **userIdentity** field in the [CloudTrail Event Reference](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/event_reference_top_level.html)\.

You can store your log files in your bucket for as long as you want, but you can also define Amazon S3 lifecycle rules to archive or delete log files automatically\. By default, your log files are encrypted by using Amazon S3 server\-side encryption \(SSE\)\.

You can choose to have CloudTrail publish Amazon SNS notifications when new log files are delivered if you want to take quick action upon log file delivery\. For more information, see [Configuring Amazon SNS Notifications](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/getting_notifications_top_level.html) in the *AWS CloudTrail User Guide*\.

You can also aggregate Systems Manager log files from multiple AWS regions and multiple AWS accounts into a single Amazon S3 bucket\. For more information, see [Aggregating CloudTrail Log Files to a Single Amazon S3 Bucket](http://docs.aws.amazon.com/awscloudtrail/latest/userguide/aggregating_logs_top_level.html) in the *AWS CloudTrail User Guide*\.

## Understanding Systems Manager Log File Entries<a name="monitoring-cloudtrail-logs-log-entries-example"></a>

CloudTrail log files can contain one or more log entries where each entry is made up of multiple JSON\-formatted events\. A log entry represents a single request from any source and includes information about the requested action, any parameters, the date and time of the action, and so on\. The log entries are not guaranteed to be in any particular order \(that is, they are not an ordered stack trace of the public API calls\)\.

The following example shows a CloudTrail log entry that demonstrates the Systems Manager delete document action on a document named `example-Document` in the US East \(Ohio\) Region \(us\-east\-2\):

```
{
    "eventVersion": "1.04",
    "userIdentity": {
        "type": "AssumedRole",
        "principalId": "AKIAI44QH8DHBEXAMPLE:203.0.113.11",
        "arn": "arn:aws:sts::123456789012:assumed-role/example-role/203.0.113.11",
        "accountId": "123456789012",
        "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
        "sessionContext": {
            "attributes": {
                "mfaAuthenticated": "false",
                "creationDate": "2018-03-06T20:19:16Z"
            },
            "sessionIssuer": {
                "type": "Role",
                "principalId": "AKIAI44QH8DHBEXAMPLE",
                "arn": "arn:aws:iam::123456789012:role/example-role",
                "accountId": "123456789012",
                "userName": "example-role"
            }
        }
    },
    "eventTime": "2018-03-06T20:30:12Z",
    "eventSource": "ssm.amazonaws.com",
    "eventName": "DeleteDocument",
    "awsRegion": "us-east-2",
    "sourceIPAddress": "203.0.113.11",
    "userAgent": "example-user-agent-string",
    "requestParameters": {
        "name": "example-Document"
    },
    "responseElements": null,
    "requestID": "86168559-75e9-11e4-8cf8-75d18EXAMPLE",
    "eventID": "832b82d5-d474-44e8-a51d-093ccEXAMPLE",
    "resources": [
        {
            "ARN": "arn:aws:ssm:us-east-2:123456789012:document/example-Document",
            "accountId": "123456789012"
        }
    ],
    "eventType": "AwsApiCall",
    "recipientAccountId": "123456789012"
}
```