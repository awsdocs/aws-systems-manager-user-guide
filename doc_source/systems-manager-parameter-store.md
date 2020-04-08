# AWS Systems Manager Parameter Store<a name="systems-manager-parameter-store"></a>

AWS Systems Manager Parameter Store provides secure, hierarchical storage for configuration data management and secrets management\. You can store data such as passwords, database strings, Amazon EC2 instance IDs, Amazon Machine Image \(AMI\) IDs, and license codes as parameter values\. You can store values as plain text or encrypted data\. You can reference Systems Manager parameters in your scripts, commands, SSM documents, and configuration and automation workflows by using the unique name that you specified when you created the parameter\. 

**Important**  
Do not store sensitive data in a `String` or `StringList` parameter\. For all sensitive data that must remain encrypted, use only the `SecureString` parameter type\.  
For more information, see [Parameter types and examples](parameter-store-about-examples.md) and [SecureString parameters](sysman-paramstore-securestring.md)\.

Parameter Store offers the following benefits and features\.
+ Use a secure, scalable, hosted secrets management service with no servers to manage\.
+ Improve your security posture by separating your data from your code\.
+ Store configuration data and encrypted strings in hierarchies and track versions\.
+ Control and audit access at granular levels\.
+ Configure change notifications and trigger automated actions for both parameters and parameter policies\.
+ Tag parameters individually, and then secure access from different levels, including operational, parameter, Amazon EC2 tag, and path levels\. 
+ Reference AWS Secrets Manager secrets by using Parameter Store parameters\.
+ Use Parameter Store parameters with other Systems Manager capabilities and AWS services to retrieve secrets and configuration data from a central store\. 

  Parameters work with Systems Manager capabilities such as Run Command, State Manager, and Automation\. You can also reference parameters in a number of other AWS services, including the following:
  + Amazon Elastic Compute Cloud \(Amazon EC2\)
  + Amazon Elastic Container Service \(Amazon ECS\)
  + AWS Secrets Manager
  + AWS Lambda
  + AWS CloudFormation
  + AWS CodeBuild
  + AWS CodePipeline
  + AWS CodeDeploy
+ Configure integration with the following AWS services for encryption, notification, monitoring, and auditing:
  + AWS Key Management Service \(AWS KMS\)
  + Amazon Simple Notification Service \(Amazon SNS\)
  + Amazon CloudWatch
  + AWS CloudTrail

**Topics**
+ [Parameter types and examples](parameter-store-about-examples.md)
+ [SecureString parameters](sysman-paramstore-securestring.md)
+ [Standard and advanced parameter tiers](parameter-store-advanced-parameters.md)
+ [Getting started with Parameter Store](sysman-paramstore-settingup.md)
+ [Working with parameters](sysman-paramstore-working.md)
+ [Using public parameters](parameter-store-public-parameters.md)
+ [Increasing Parameter Store throughput](parameter-store-throughput.md)
+ [Specifying a default parameter tier](ps-default-tier.md)
+ [Parameter Store walkthroughs](sysman-paramstore-walk.md)