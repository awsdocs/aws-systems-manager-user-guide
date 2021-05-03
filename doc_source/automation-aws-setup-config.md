# AWSSupport\-SetupConfig<a name="automation-aws-setup-config"></a>

**Description**

The AWSSupport\-SetupConfig runbook creates an AWS Identity and Access Management \(IAM\) service\-linked role, a configuration recorder powered by AWS Config, and a delivery channel with an Amazon Simple Storage Service \(Amazon S3\) bucket where AWS Config sends configuration snapshots and configuration history files\. If you specify values for the `AggregatorAccountId` and `AggregatorAccountRegion` parameters, the runbook also creates authorizations for data aggregation to collect AWS Config configuration and compliance data from multiple AWS accounts and multiple AWS Regions\. To learn more about aggregating data from multiple accounts and Regions, see [Multi\-Account Multi\-Region Data Aggregation](https://docs.aws.amazon.com/config/latest/developerguide/aggregate-data.html) in the *AWS Config Developer Guide*\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-SetupConfig)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this runbook\.
+ AggregatorAccountId

  Type: String

  Description: \(Optional\) The ID of the AWS account where an aggregator will be added to aggregate AWS Config configuration and compliance data from multiple accounts and AWS Regions\. This account is also used by the aggregator to authorize the source accounts\.
+ AggregatorAccountRegion

  Type: String

  Description: \(Optional\) The Region where an aggregator will be added to aggregate AWS Config configuration and compliance data from multiple accounts and Regions\.
+ IncludeGlobalResourcesRegion

  Type: String

  Default: us\-east\-1

  Description: \(Required\) To avoid recording global resource data in each Region, specify one Region to record global resource data from\.
+ Partition

  Type: String

  Default: aws

  Description: \(Required\) The partition you want to collect AWS Config configuration and compliance data from\. For AWS GovCloud, use `aws-us-gov`\.
+ S3BucketName

  Type: String

  Default: aws\-config\-delivery\-channel

  Description: \(Optional\) The name you want to apply to the Amazon S3 bucket created for the delivery channel\. The account ID is appended to the end of the name\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `config:DescribeConfigurationRecorders`
+ `config:DescribeDeliveryChannels`
+ `config:PutAggregationAuthorization`
+ `config:PutConfigurationRecorder`
+ `config:PutDeliveryChannel`
+ `config:StartConfigurationRecorder`
+ `iam:CreateServiceLinkedRole`
+ `iam:PassRole`
+ `s3:CreateBucket`
+ `s3:PutBucketPolicy`

**Document Steps**
+ aws:executeScript \- Creates a service\-linked IAM role for AWS Config if one does not already exist\.
+ aws:executeScript \- Creates a configuration recorder if one does not already exist\.
+ aws:executeScript \- Creates an Amazon S3 bucket to be used by the delivery channel if one does not already exist\.
+ aws:executeScript \- Creates a delivery channel using the resources created by the runbook\.
+ aws:executeAwsApi \- Starts the configuration recorder\.
+ aws:executeScript \- If you specified values for the `AggregatorAccountId` and `AggregatorAccountRegion` parameters, authorizations for multi\-account and multi\-Region data aggregation are configured\.