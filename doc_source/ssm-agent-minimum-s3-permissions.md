# Minimum S3 Bucket Permissions for SSM Agent<a name="ssm-agent-minimum-s3-permissions"></a>

This topic provides information about the Amazon Simple Storage Service \(Amazon S3\) buckets that resources might need to access to perform Systems Manager operations\. You can specify these buckets in a custom policy if you want to limit Amazon S3 bucket access for an instance profile or VPC endpoint to the minimum required to use Systems Manager\.

These permissions only provide access to the resources required by the SSM Agent\. They don't reflect permissions needed for other Amazon S3\-related operations\. 

**Topics**
+ [Required Permissions](#ssm-agent-minimum-s3-permissions-required)
+ [Example](#ssm-agent-minimum-s3-permissions-example)

## Required Permissions<a name="ssm-agent-minimum-s3-permissions-required"></a>

The following table describes each of the Amazon S3 bucket policy permissions needed for using Systems Manager\.

Amazon S3 permissions required by SSM Agent


| Permission | Description | 
| --- | --- | 
| arn:aws:s3:::aws\-ssm\-region/\* |  Provides access to the Amazon S3 bucket containing modules required for use with SSM documents\.  | 
| arn:aws:s3:::aws\-windows\-downloads\-region/\* |  Required for some SSM documents that support Windows operating systems\.  | 
| arn:aws:s3:::amazon\-ssm\-packages\-region/\* |  Required for using versions of the SSM Agent prior to 2\.2\.45\.0 to run the document `AWS-ConfigureAWSPackage`\.  | 
| arn:aws:s3:::region\-birdwatcher\-prod/\* |  Provides access to the distribution service used by version 2\.2\.45\.0 and later of the SSM Agent\. This service is used to run the document `AWS-ConfigureAWSPackage`\.  | 

*region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

## Example<a name="ssm-agent-minimum-s3-permissions-example"></a>

The following example illustrates how to provide access to the Amazon S3 buckets required for Systems Manager operations in the US East \(Ohio\) Region \(us\-east\-2\)\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": [
                "arn:aws:s3:::aws-ssm-us-east-2/*",
                "arn:aws:s3:::aws-windows-downloads-us-east-2/*",
                "arn:aws:s3:::amazon-ssm-packages-us-east-2/*",
                "arn:aws:s3:::us-east-2-birdwatcher-prod/*"
            ]
        }
    ]
}
```

**Important**  
We recommend that you avoid using wildcard characters \(\*\) in place of specific Regions in this policy, such as `arn:aws:s3:::aws-ssm-*/*` instead of `arn:aws:s3:::aws-ssm-us-east-2/*`\. Doing so could provide access to other Amazon S3 buckets to which you did not intend to grant permissions\.