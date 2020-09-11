# AWSSupport\-ListEC2Resources<a name="automation-awssupport-listec2resources"></a>

 **Description** 

The AWSSupport\-ListEC2Resources Automation document returns information on Amazon EC2 instances and related resources like Amazon Elastic Block Store \(Amazon EBS\) volumes, Elastic IP addresses, and Amazon EC2 Auto Scaling groups from the AWS Regions you specify\. By default, the information is gathered from all Regions and is displayed in the output of the automation\. Optionally, you can specify an Amazon Simple Storage Service \(Amazon S3\) bucket for the information to be uploaded to as a comma\-separated values \(\.csv\) file\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-ListEC2Resources)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ Bucket

  Type: String

  Description: \(Optional\) The name of the S3 bucket where the information gathered is uploaded to\.
+ DisplayResourceDeletionDocumentation

  Type: String

  Default: True

  Description: \(Optional\) If set to `True`, the automation creates links in the output to documentation related to deleting your resources\.
+ RegionsToQuery

  Type: String

  Default: All

  Description: \(Optional\) The Regions you want to gather Amazon EC2 related information from\.

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `autoscaling:DescribeAutoScalingGroups`
+ `ec2:DescribeAddresses`
+ `ec2:DescribeImages`
+ `ec2:DescribeInstances`
+ `ec2:DescribeNetworkInterfaces`
+ `ec2:DescribeRegions`
+ `ec2:DescribeVolumes`
+ `ec2:DescribeSnapshots`
+ `elasticloadbalancing:DescribeLoadBalancers`

Additionally, to successfully upload the information gathered to the S3 bucket you specify, the `AutomationAssumeRole` requires the following actions:
+ `s3:GetBucketAcl`
+ `s3:GetBucketPolicyStatus`
+ `s3:PutObject`

**Document Steps**
+ aws:executeAwsApi \- Gathers the Regions enabled for the account\.
+ aws:executeScript \- Confirms the Regions enabled for the account support the Regions specified in the `RegionsToQuery` parameter\.
+ aws:branch \- If no Regions are enabled for the account, the automation ends\.
+ aws:executeScript \- Lists all EC2 instances for the account and Regions you specify\.
+ aws:executeScript \- Lists all Amazon Machine Images \(AMI\) for the account and Regions you specify\.
+ aws:executeScript \- Lists all EBS volumes for the account and Regions you specify\.
+ aws:executeScript \- Lists all Elastic IP addresses for the account and Regions you specify\.
+ aws:executeScript \- Lists all elastic network interfaces for the account and Regions you specify\.
+ aws:executeScript \- Lists all Auto Scaling groups for the account and Regions you specify\.
+ aws:executeScript \- Lists all load balancers for the account and Regions you specify\.
+ aws:executeScript \- Uploads the information gathered to the S3 bucket specified if you provide a value for the `Bucket` parameter\.