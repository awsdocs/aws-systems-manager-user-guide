# Data protection in AWS Systems Manager<a name="data-protection"></a>

AWS Systems Manager conforms to the AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/), which includes regulations and guidelines for data protection\. AWS is responsible for protecting the global infrastructure that runs all the AWS services\. AWS maintains control over data hosted on this infrastructure, including the security configuration controls for handling customer content and personal data\. AWS customers and APN partners, acting either as data controllers or data processors, are responsible for any personal data that they put in the AWS Cloud\. 

Data protection refers to protecting data while *in transit* \(as it travels to and from Systems Manager\) and *at rest* \(while it is stored in AWS data centers\)\. 

For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\), so that each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3\.

We strongly recommend that you never put sensitive identifying information, such as your customers' account numbers, into free\-form fields such as a **Name** field\. This includes when you work with Systems Manager or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into Systems Manager or other services might get picked up for inclusion in diagnostic logs\. When you provide a URL to an external server, don't include credentials information in the URL to validate your request to that server\.

For more information about data protection, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

## Data encryption<a name="data-encryption"></a>

### Encryption at rest<a name="encryption-at-rest"></a>

**Parameter Store Parameters**  
The types of parameters you can create in Parameter Store include `String`, `StringList`, and `SecureString`\.

To encrypt `SecureString` parameter values, Parameter Store uses an AWS Key Management Service \(AWS KMS\) customer master key \(CMK\)\. AWS KMS uses either a customer managed CMK or an AWS managed CMK to encrypt the parameter value in an AWS managed database\.

**Important**  
Do not store sensitive data in a `String` or `StringList` parameter\. For all sensitive data that must remain encrypted, use only the `SecureString` parameter type\.  
For more information, see [Parameter types and examples](parameter-store-about-examples.md) and [SecureString parameters](sysman-paramstore-securestring.md)\.

**Content in Amazon S3 Buckets**  
As part of your Systems Manager operations, you might choose to upload or store data in one or more Amazon Simple Storage Service \(Amazon S3\) buckets\. 

For information about S3 bucket encryption, see [Protecting Data Using Encryption](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html) and [Data Protection in Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/DataDurability.html) in the *Amazon Simple Storage Service Developer Guide*\.

The following are types of data you can upload or have stored in S3 buckets as part of your Systems Manager activities\.
+ The output of Run Command commands
+ Distributor packages
+ Patch Manager patching operation logs
+ Patch Manager patch override lists
+ Scripts or Ansible Playbooks to run in an Automation document workflow
+ Chef InSpec profiles for use with Compliance scans
+ AWS CloudTrail logs
+ Session Manager session history logs
+ Reports from Explorer or OpsData from OpsCenter
+ AWS CloudFormation templates for use with Automation workflows
+ Compliance data from a Resource Data Sync scan
+ Output of requests to create or edit State Manager association on managed instances
+ Custom SSM documents that you can run using the AWS managed `AWS-RunDocument` document

**CloudWatch Logs Log Groups**  
As part of your Systems Manager operations, you might choose to stream data to one or more Amazon CloudWatch Logs log groups\.

For information about CloudWatch Logs log group encryption, see [Encrypt Log Data in CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/encrypt-log-data-kms.html) in the *Amazon CloudWatch Logs User Guide*\.

The following are types of data you might have streamed to a CloudWatch Logs log group as part of your Systems Manager activities\.
+ The output of Run Command commands
+ The output of scripts run using the `aws:executeScript` action in an Automation document
+ Session Manager session history logs
+ Logs from SSM Agent on your managed instances

### Encryption in transit<a name="encryption-in-transit"></a>

We recommend that you use an encryption protocol such as Transport Layer Security \(TLS\) to encrypt sensitive data in transit between clients and your instances\.

Systems Manager provides the following support for encryption of your data in transit\.

**Connections to Systems Manager API endpoints**  
Systems Manager API endpoints only support secure connections over HTTPS\. When you manage Systems Manager resources with the AWS Management Console, AWS SDK, or the Systems Manager API, all communication is encrypted with Transport Layer Security \(TLS\)\. For a full list of API endpoints, see [Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the *Amazon Web Services General Reference*\. 

**Managed instances**  
AWS provides secure and private connectivity between EC2 instances\. In addition, we automatically encrypt in\-transit traffic between supported instances in the same VPC or in peered VPCs, using AEAD algorithms with 256\-bit encryption\. This encryption feature uses the offload capabilities of the underlying hardware, and there is no impact on network performance\. The supported instances are: C5n, G4, I3en, M5dn, M5n, P3dn, R5dn, and R5n\.

**Session Manager sessions**  
By default, Session Manager uses TLS 1\.2 to encrypt session data transmitted between the local machines of users in your account and your EC2 instances\. You can also choose to further encrypt the data in transit using a customer master key \(CMK\) that has been created in AWS Key Management Service\. 

**Run Command access**  
By default, remote access to your instances using Run Command is encrypted using TLS 1\.2, and requests to create a connection are signed using SigV4\.

## Internetwork traffic privacy<a name="internetwork-privacy"></a>

You can use Amazon Virtual Private Cloud \(Amazon VPC\) to create boundaries between resources in your managed instances and control traffic between them, your on\-premises network, and the internet\. For details, see [\(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md)\. 

For more information about Amazon Virtual Private Cloud security, see [Security](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html) in the *Amazon VPC User Guide*\.