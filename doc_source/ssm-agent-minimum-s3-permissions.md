# SSM Agent communications with AWS managed S3 buckets<a name="ssm-agent-minimum-s3-permissions"></a>

In the course of performing various Systems Manager operations, AWS Systems Manager Agent \(SSM Agent\) accesses a number of Amazon Simple Storage Service \(Amazon S3\) buckets\. These S3 buckets are publicly accessible, and by default, SSM Agent connects to them using `HTTP` calls\. 

However, if you're using a virtual private cloud \(VPC\) endpoint in your Systems Manager operations, you must provide explicit permission in an Amazon Elastic Compute Cloud \(Amazon EC2\) instance profile for Systems Manager, or in a service role for instances in a hybrid environment\. Otherwise, your resources can't access these public buckets\.

To grant your managed instances access to these buckets when you are using a VPC endpoint, you create a custom Amazon S3 permissions policy, and then attach it to your instance profile \(for EC2 instances\) or your service role \(for on\-premises servers and virtual machines in a hybrid environment\)\.

**Note**  
These permissions only provide access to the AWS managed buckets required by SSM Agent\. They don't provide the permissions that are necessary for other Amazon S3 operations\. They also don't provide permission to your own S3 buckets\. 

For more information, see the following topics: 
+ [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)
+ [Create an IAM service role for a hybrid environment](sysman-service-role.md)

**Topics**
+ [Required bucket permissions](#ssm-agent-minimum-s3-permissions-required)
+ [Example](#ssm-agent-minimum-s3-permissions-example)

## Required bucket permissions<a name="ssm-agent-minimum-s3-permissions-required"></a>

The following table describes each of the S3 buckets that SSM Agent might need to access for Systems Manager operations\.

**Note**  
*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

Amazon S3 permissions required by SSM Agent


| S3 bucket ARN | Description | 
| --- | --- | 
|  `arn:aws:s3:::aws-windows-downloads-region/*`  |  Required for some SSM documents that support only Windows operating systems\.  | 
|  `arn:aws:s3:::amazon-ssm-region/*`  | Required for updating SSM Agent installations\. These buckets contain the SSM Agent installation packages, and the installation manifests that are referenced by the AWS\-UpdateSSMAgent document and plugin\. If these permissions aren't provided, the SSM Agent makes an HTTP call to download the update\.  | 
|  `arn:aws:s3:::amazon-ssm-packages-region/*`  |  Required for using versions of SSM Agent prior to 2\.2\.45\.0 to run the SSM document `AWS-ConfigureAWSPackage`\.  | 
|  `arn:aws:s3:::region-birdwatcher-prod/*`  |  Provides access to the distribution service used by version 2\.2\.45\.0 and later of SSM Agent\. This service is used to run the document `AWS-ConfigureAWSPackage`\.  This permission is needed for all AWS Regions *except* the Africa \(Cape Town\) Region \(af\-south\-1\) and the Europe \(Milan\) Region \(eu\-south\-1\)\.  | 
|  `arn:aws:s3:::aws-ssm-distributor-file-region/*`  |  Provides access to the distribution service used by version 2\.2\.45\.0 and later of SSM Agent\. This service is used to run the SSM document `AWS-ConfigureAWSPackage`\.  This permission is needed *only* for the Africa \(Cape Town\) Region \(af\-south\-1\) and the Europe \(Milan\) Region \(eu\-south\-1\)\.  | 
|  `arn:aws:s3:::aws-ssm-document-attachments-region/*`  |  Provides access to the S3 bucket containing the packages for Distributor, a capability of AWS Systems Manager, that are owned by AWS\.  | 
|  `arn:aws:s3:::patch-baseline-snapshot-region/*`  |  Provides access to the S3 bucket containing patch baseline snapshots\. This is required if you use any of the following SSM documents: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent-minimum-s3-permissions.html)  In the Middle East \(Bahrain\) Region \(me\-south\-1\) only, this S3 bucket uses a different naming convention\. For this AWS Region only, use the following bucket instead\.   `patch-baseline-snapshot-me-south-1-uduvl7q8`   In the Africa \(Cape Town\) Region \(af\-south\-1\) only, this S3 bucket uses a different naming convention\. For this AWS Region only, use the following bucket instead\.   `patch-baseline-snapshot-af-south-1-tbxdb5b9`     | 
|  For Linux and Windows Server instances: `arn:aws:s3:::aws-ssm-region/*` For Amazon EC2 instances for macOS: `arn:aws:s3:::aws-patchmanager-macos-region/*`  |  Provides access to the S3 bucket containing modules required for use with certain Systems Manager documents \(SSM documents\)\. For example:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent-minimum-s3-permissions.html)  Exceptions The S3 bucket names in a few AWS Regions use an extended naming convention, as shown by their ARNs\. For these Regions, use the following ARNs instead:  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent-minimum-s3-permissions.html)  SSM documents The following are some commonly used SSM documents stored in these buckets\.  In `arn:aws:s3:::aws-ssm-region/`: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent-minimum-s3-permissions.html) In `arn:aws:s3:::aws-patchmanager-macos-region/`: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/ssm-agent-minimum-s3-permissions.html)  | 

## Example<a name="ssm-agent-minimum-s3-permissions-example"></a>

The following example illustrates how to provide access to the S3 buckets required for Systems Manager operations in the US East \(Ohio\) Region \(us\-east\-2\)\. In most cases, you need to provide these permissions explicitly in an instance profile or service role only when using a VPC endpoint\.

**Important**  
We recommend that you avoid using wildcard characters \(\*\) in place of specific Regions in this policy\. For example, use `arn:aws:s3:::aws-ssm-us-east-2/*` and don't use `arn:aws:s3:::aws-ssm-*/*`\. Using wildcards could provide access to S3 buckets that you don’t intend to grant access to\. If you want to use the instance profile for more than one Region, we recommend repeating the first `Statement` block for each Region\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": [
                "arn:aws:s3:::aws-windows-downloads-us-east-2/*",
                "arn:aws:s3:::amazon-ssm-us-east-2/*",
                "arn:aws:s3:::amazon-ssm-packages-us-east-2/*",
                "arn:aws:s3:::us-east-2-birdwatcher-prod/*",
                "arn:aws:s3:::aws-ssm-document-attachments-us-east-2/*",
                "arn:aws:s3:::patch-baseline-snapshot-us-east-2/*",
                "arn:aws:s3:::aws-ssm-us-east-2/*",
                "arn:aws:s3:::aws-patchmanager-macos-us-east-2/*"
            ]
        }
    ]
}
```