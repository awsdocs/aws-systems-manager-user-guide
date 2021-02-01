# AWSSupport\-TroubleshootS3PublicRead<a name="automation-awssupport-troubleshoots3publicread"></a>

 **Description** 

The AWSSupport\-TroubleshootS3PublicRead runbook diagnoses issues reading objects from the public Amazon Simple Storage Service \(Amazon S3\) bucket you specify in the `S3BucketName` parameter\. A subset of settings are also analyzed for objects in the S3 bucket\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-TroubleshootS3PublicRead)

**Limitations**
+ This automation does not check for access points that allow public access to objects\.
+ This automation does not evaluate condition keys in the S3 bucket policy\.
+ If you're using AWS Organizations, this automation does not evaluate service control policies to confirm that access to Amazon S3 is allowed\.

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ CloudWatchLogGroupName

  Type: String

  Description: \(Optional\) The Amazon CloudWatch Logs log group where you want to send the automation output\. If a log group is not found that matches the value you specify, the automation will create a log group using this parameter value\. The retention period for the log group created by this automation is 14 days\.
+ CloudWatchLogStreamName

  Type: String

  Description: \(Optional\) The CloudWatch Logs log stream where you want to send the automation output\. If a log stream is not found that matches the value you specify, the automation will create a log stream using this parameter value\. If you do not specify a value for this parameter, the automation will use the `ExecutionId` for the name of the log stream\.
+ HttpGet

  Type: Boolean

  Valid values: true \| false

  Default: true

  Description: \(Optional\) If this parameter is set to `true`, the automation makes a partial HTTP request to the objects in the `S3BucketName` you specify\. Only the first byte of the object is returned using the Range HTTP header\.
+ IgnoreBlockPublicAccess

  Type: Boolean

  Valid values: true \| false

  Default: false

  Description: \(Optional\) If this parameter is set to `true`, the automation ignores the public access block settings of the S3 bucket you specify in the `S3BucketName` parameter\. Changing this parameter from the default value is not recommended\.
+ MaxObjects

  Type: Integer

  Allowed values: 1\-25

  Default: 5

  Description: \(Optional\) The number of objects to analyze in the S3 bucket you specify in the `S3BucketName` parameter\.
+ S3BucketName

  Type: String

  Description: \(Required\) The name of the S3 bucket to troubleshoot\.
+ S3PrefixName

  Type: String

  Description: \(Optional\) The key name prefix of the objects you want to analyze in your S3 bucket\. For more information, see [Object keys](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html#object-keys) in the *Amazon Simple Storage Service Developer Guide*\.
+ StartAfter

  Type: String

  Description: \(Optional\) The object key name where you want the automation to begin analyzing objects in your S3 bucket\.
+ ResourcePartition

  Type: String

  Allowed values: aws \| aws\-us\-gov \| aws\-cn

  Default: aws

  Description: \(Required\) The partition where your S3 bucket is located\.
+ Verbose

  Type: Boolean

  Valid values: true \| false

  Default: false

  Description: \(Optional\) To return more detailed information during the automation, set this parameter to `true`\. Only warning and error messages will be returned if the parameter is set to `false`\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.

The `logs:CreateLogGroup`, `logs:CreateLogStream`, and `logs:PutLogEvents` permissions are only required if you want the automation to send log data to CloudWatch Logs\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "iam:SimulateCustomPolicy",
                "iam:GetContextKeysForCustomPolicy",
                "s3:ListAllMyBuckets",
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:PutRetentionPolicy",
                "s3:GetAccountPublicAccessBlock"
            ],
            "Resource": "*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "s3:GetObject",
                "s3:GetObjectAcl",
                "s3:GetObjectTagging"
            ],
            "Resource": "arn:aws:s3:::awsexamplebucket1/*",
            "Effect": "Allow"
        },
        {
            "Action": [
                "s3:ListBucket",
                "s3:GetBucketLocation",
                "s3:GetBucketPublicAccessBlock",
                "s3:GetBucketRequestPayment",
                "s3:GetBucketPolicyStatus",
                "s3:GetBucketPolicy",
                "s3:GetBucketAcl"
            ],
            "Resource": "arn:aws:s3:::awsexamplebucket1",
            "Effect": "Allow"
        }
    ]
}
```

**Document Steps**
+ aws:assertAwsResourceProperty \- Confirms the S3 bucket exists, and is accessible\.
+ aws:executeScript \- Returns the S3 bucket location and your canonical user ID\.
+ aws:executeScript \- Returns the public access block settings for your account and the S3 bucket\.
+ aws:assertAwsResourceProperty \- Confirms the S3 bucket payer is set to `BucketOwner`\. If `Requester Pays` is enabled on the S3 bucket, the automation ends\.
+ aws:executeScript \- Returns the S3 bucket policy status and determines whether it is considered public\. For more information about public S3 buckets, see [The meaning of "public"](https://docs.aws.amazon.com/AmazonS3/latest/dev/access-control-block-public-access.html#access-control-block-public-access-policy-status) in the *Amazon Simple Storage Service Developer Guide*\.
+ aws:executeAwsApi \- Returns the S3 bucket policy\.
+ aws:executeAwsApi \- Returns all context keys found in the S3 bucket policy\.
+ aws:assertAwsResourceProperty \- Confirms whether there is an explicit deny in the S3 bucket policy for the `GetObject` API action\.
+ aws:executeAwsApi \- Returns the access control list \(ACL\) for the S3 bucket\.
+ aws:executeScript \- Creates a CloudWatch Logs log group and log stream if you specify a value for the `CloudWatchLogGroupName` parameter\.
+ aws:executeScript \- Based on the values you specify in the runbook input parameters, evaluates whether any of the S3 bucket settings gathered during the automation are preventing objects from being accessed by the public\. This script performs the following functions:
  + Evaluates public access block settings
  + Returns objects from your S3 bucket based on the values you specify in the `MaxObjects`, `S3PrefixName`, and `StartAfter` parameters\.
  + Returns the S3 bucket policy to simulate a custom IAM policy for the objects returned from your S3 bucket\.
  + Performs a partial HTTP request to the returned objects if the `HttpGet` parameter is set to `true`\. Only the first byte of the object is returned using the Range HTTP header\.
  + Checks the returned object's key name to confirm whether it ends with one or two periods\. Object key names that end in periods can't be downloaded from the Amazon S3 console\.
  + Checks whether the returned object's owner matches the owner of the S3 bucket\.
  + Checks whether the object's ACL grants `READ` or `FULL_CONTROL` permissions to anonymous users\.
  + Returns tags associated with the object\.
  + Uses the simulated IAM policy to confirm whether there is an explicit deny for this object in the S3 bucket policy for the `GetObject` API action\.
  + Returns the object's metadata to confirm that the storage class is supported\.
  + Checks the object's server\-side encryption settings to confirm whether the object is encrypted using a AWS Key Management Service \(AWS KMS\) customer master key\.

 **Outputs** 

AnalyzeObjects\.bucket

AnalyzeObjects\.object