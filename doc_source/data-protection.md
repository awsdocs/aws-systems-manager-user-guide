# Data protection in AWS Systems Manager<a name="data-protection"></a>

Data protection refers to protecting data while *in transit* \(as it travels to and from Systems Manager\) and *at rest* \(while it's stored in AWS data centers\)\.

The AWS [shared responsibility model](http://aws.amazon.com/compliance/shared-responsibility-model/) applies to data protection in AWS Systems Manager\. As described in this model, AWS is responsible for protecting the global infrastructure that runs all of the AWS Cloud\. You are responsible for maintaining control over your content that is hosted on this infrastructure\. This content includes the security configuration and management tasks for the AWS services that you use\. For more information about data privacy, see the [Data Privacy FAQ](http://aws.amazon.com/compliance/data-privacy-faq)\. For information about data protection in Europe, see the [AWS Shared Responsibility Model and GDPR](http://aws.amazon.com/blogs/security/the-aws-shared-responsibility-model-and-gdpr/) blog post on the *AWS Security Blog*\.

For data protection purposes, we recommend that you protect AWS account credentials and set up individual user accounts with AWS Identity and Access Management \(IAM\)\. That way each user is given only the permissions necessary to fulfill their job duties\. We also recommend that you secure your data in the following ways:
+ Use multi\-factor authentication \(MFA\) with each account\.
+ Use SSL/TLS to communicate with AWS resources\. We recommend TLS 1\.2 or later\.
+ Set up API and user activity logging with AWS CloudTrail\.
+ Use AWS encryption solutions, along with all default security controls within AWS services\.
+ Use advanced managed security services such as Amazon Macie, which assists in discovering and securing personal data that is stored in Amazon S3\.
+ If you require FIPS 140\-2 validated cryptographic modules when accessing AWS through a command line interface or an API, use a FIPS endpoint\. For more information about the available FIPS endpoints, see [Federal Information Processing Standard \(FIPS\) 140\-2](http://aws.amazon.com/compliance/fips/)\.

We strongly recommend that you never put confidential or sensitive information, such as your customers' email addresses, into tags or free\-form fields such as a **Name** field\. This includes when you work with Systems Manager or other AWS services using the console, API, AWS CLI, or AWS SDKs\. Any data that you enter into tags or free\-form fields used for names may be used for billing or diagnostic logs\. If you provide a URL to an external server, we strongly recommend that you do not include credentials information in the URL to validate your request to that server\.

## Data encryption<a name="data-encryption"></a>

### Encryption at rest<a name="encryption-at-rest"></a>

**Parameter Store parameters**  
The types of parameters you can create in Parameter Store, a capability of AWS Systems Manager, include `String`, `StringList`, and `SecureString`\.

To encrypt `SecureString` parameter values, Parameter Store uses an AWS KMS key in AWS Key Management Service \(AWS KMS\)\. AWS KMS uses either a customer managed key or an AWS managed key to encrypt the parameter value in an AWS managed database\.

**Important**  
Don't store sensitive data in a `String` or `StringList` parameter\. For all sensitive data that must remain encrypted, use only the `SecureString` parameter type\.  
For more information, see [What is a parameter?](systems-manager-parameter-store.md#what-is-a-parameter) and [Restricting access to Systems Manager parameters using IAM policies](sysman-paramstore-access.md)\.

**Content in Amazon S3 buckets**  
As part of your Systems Manager operations, you might choose to upload or store data in one or more Amazon Simple Storage Service \(Amazon S3\) buckets\. 

For information about S3 bucket encryption, see [Protecting data using encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingEncryption.html) and [Data protection in Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/DataDurability.html) in the *Amazon Simple Storage Service User Guide*\.

The following are types of data you can upload or have stored in S3 buckets as part of your Systems Manager activities:
+ The output of commands in Run Command, a capability of AWS Systems Manager
+ Packages in Distributor, a capability of AWS Systems Manager
+ Patching operation logs in Patch Manager, a capability of AWS Systems Manager
+ Patch Manager patch override lists
+ Scripts or Ansible Playbooks to run in a runbook workflow in Automation, a capability of AWS Systems Manager 
+ Chef InSpec profiles for use with scans in Compliance, a capability of AWS Systems Manager
+ AWS CloudTrail logs
+ Session history logs in Session Manager, a capability of AWS Systems Manager
+ Reports from Explorer, , a capability of AWS Systems Manager
+ OpsData from OpsCenter, a capability of AWS Systems Manager
+ AWS CloudFormation templates for use with Automation workflows
+ Compliance data from a resource data sync scan
+ Output of requests to create or edit association in State Manager, a capability of AWS Systems Manager, on managed nodes
+ Custom Systems Manager documents \(SSM documents\) that you can run using the AWS managed SSM document `AWS-RunDocument`

**CloudWatch Logs log groups**  
As part of your Systems Manager operations, you might choose to stream data to one or more Amazon CloudWatch Logs log groups\.

For information about CloudWatch Logs log group encryption, see [Encrypt log data in CloudWatch Logs using AWS Key Management Service](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/encrypt-log-data-kms.html) in the *Amazon CloudWatch Logs User Guide*\.

The following are types of data you might have streamed to a CloudWatch Logs log group as part of your Systems Manager activities:
+ The output of Run Command commands
+ The output of scripts run using the `aws:executeScript` action in an Automation runbook
+ Session Manager session history logs
+ Logs from SSM Agent on your managed nodes

### Encryption in transit<a name="encryption-in-transit"></a>

We recommend that you use an encryption protocol such as Transport Layer Security \(TLS\) to encrypt sensitive data in transit between clients and your nodes\.

Systems Manager provides the following support for encryption of your data in transit\.

**Connections to Systems Manager API endpoints**  
Systems Manager API endpoints only support secure connections over HTTPS\. When you manage Systems Manager resources with the AWS Management Console, AWS SDK, or the Systems Manager API, all communication is encrypted with Transport Layer Security \(TLS\)\. For a full list of API endpoints, see [AWS service endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html) in the *Amazon Web Services General Reference*\. 

**Managed instances**  
AWS provides secure and private connectivity between Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. In addition, we automatically encrypt in\-transit traffic between supported instances in the same virtual private cloud \(VPC\) or in peered VPCs, using AEAD algorithms with 256\-bit encryption\. This encryption feature uses the offload capabilities of the underlying hardware, and there is no impact on network performance\. The supported instances are: C5n, G4, I3en, M5dn, M5n, P3dn, R5dn, and R5n\.

**Session Manager sessions**  
By default, Session Manager uses TLS 1\.2 to encrypt session data transmitted between the local machines of users in your account and your EC2 instances\. You can also choose to further encrypt the data in transit using an AWS KMS key that has been created in AWS KMS\. 

**Run Command access**  
By default, remote access to your nodes using Run Command is encrypted using TLS 1\.2, and requests to create a connection are signed using SigV4\.

## Internetwork traffic privacy<a name="internetwork-privacy"></a>

You can use Amazon Virtual Private Cloud \(Amazon VPC\) to create boundaries between resources in your managed nodes and control traffic between them, your on\-premises network, and the internet\. For details, see [\(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md)\. 

For more information about Amazon Virtual Private Cloud security, see [Internetwork traffic privacy in Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html) in the *Amazon VPC User Guide*\.