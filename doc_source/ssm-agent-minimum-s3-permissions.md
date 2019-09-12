# About Minimum S3 Bucket Permissions for SSM Agent<a name="ssm-agent-minimum-s3-permissions"></a>

This topic provides information about the Amazon Simple Storage Service \(Amazon S3\) buckets that SSM Agent might need to access to in order to perform Systems Manager operations\. These buckets are publicly accessible, but in some cases, you might need to provide explicit permission in an EC2 instance profile for Systems Manager, or in a service role for instances in a hybrid environment\. Most commonly, you must grant these permissions if you are using a private VPC endpoint in your Systems Manager operations\. Otherwise, your resources can't access these public buckets\. 

To grant access to these buckets, you create a custom S3 permissions policy, and then attach it to your instance profile \(for EC2 instances\) or your service role \(for on\-premises servers and virtual machines \(VMs\) in a hybrid environment\.

**Note**  
These permissions only provide access to the AWS managed buckets required by SSM Agent\. They don't provide the permissions that are necessary for other Amazon S3 operations\. They also don't provide permission to your own S3 buckets\. 

For more information, see the following topics: 
+ [Create an IAM Instance Profile for Systems Manager](setup-instance-profile.md)
+ [Create an IAM Service Role for a Hybrid Environment](sysman-service-role.md)

**Topics**
+ [Required Permissions](#ssm-agent-minimum-s3-permissions-required)
+ [Example](#ssm-agent-minimum-s3-permissions-example)

## Required Permissions<a name="ssm-agent-minimum-s3-permissions-required"></a>

The following table describes each of the Amazon S3 policy permissions needed for using Systems Manager\.

Amazon S3 permissions required by SSM Agent


| Permission | Description | 
| --- | --- | 
| arn:aws:s3:::aws\-ssm\-region/\* |  Provides access to the Amazon S3 bucket containing modules required for use with SSM documents\.  | 
| arn:aws:s3:::aws\-windows\-downloads\-region/\* |  Required for some SSM documents that support Windows operating systems\.  | 
| arn:aws:s3:::amazon\-ssm\-region/\* | Required for updating SSM Agent installations\. These buckets contain the SSM Agent installation packages, and the installation manifests that are referenced by the AWS\-UpdateSSMAgent document and plugin\.  | 
| arn:aws:s3:::amazon\-ssm\-packages\-region/\* |  Required for using versions of SSM Agent prior to 2\.2\.45\.0 to run the document `AWS-ConfigureAWSPackage`\.  | 
| arn:aws:s3:::region\-birdwatcher\-prod/\* |  Provides access to the distribution service used by version 2\.2\.45\.0 and later of SSM Agent\. This service is used to run the document `AWS-ConfigureAWSPackage`\.  | 
| arn:aws:s3:::patch\-baseline\-snapshot\-region/\* |  Provides access to the Amazon S3 bucket containing patch baseline snapshots\. This is required if you use the `AWS-RunPatchBaseline` and `AWS-ApplyPatchBaseline` documents\.  | 

*region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager Table of Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

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
                "arn:aws:s3:::amazon-ssm-us-east-2/*",
                "arn:aws:s3:::amazon-ssm-packages-us-east-2/*",
                "arn:aws:s3:::us-east-2-birdwatcher-prod/*",
                "arn:aws:s3:::patch-baseline-snapshot-us-east-2/*"
            ]
        }
    ]
}
```

**Important**  
We recommend that you avoid using wildcard characters \(\*\) in place of specific Regions in this policy\. For example, use `arn:aws:s3:::aws-ssm-us-east-2/*` and do not use `arn:aws:s3:::aws-ssm-*/*`\. Using wildcards could provide access to Amazon S3 buckets that you don’t intend to grant access to\. If you want to use the instance profile for more than one Region, we recommend repeating the first `Statement` block for each Region\.