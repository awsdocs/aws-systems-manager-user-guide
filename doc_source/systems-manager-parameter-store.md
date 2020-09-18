# AWS Systems Manager Parameter Store<a name="systems-manager-parameter-store"></a>

AWS Systems Manager Parameter Store provides secure, hierarchical storage for configuration data management and secrets management\. You can store data such as passwords, database strings, Amazon Machine Image \(AMI\) IDs, and license codes as parameter values\. You can store values as plain text or encrypted data\. You can reference Systems Manager parameters in your scripts, commands, SSM documents, and configuration and automation workflows by using the unique name that you specified when you created the parameter\. 

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
+ Validation of ID format when you specify an Amazon Machine Image \(AMI\) ID as a parameter value\.
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
  + Amazon EventBridge
  + AWS CloudTrail

**Tagging a parameter**  
You can tag your parameters to help you quickly identify one or more parameters based on the tags you've assigned to them\. For example, you can tag parameters for specific environments, departments, users, groups, or periods\. You can also restrict access to parameters by creating an IAM policy that specifies the tags that a user or group can access\. For more information, see [Tagging Systems Manager parameters](tagging-parameters.md)\.

**Amazon EventBridge support**  
This Systems Manager capability is supported as an *event* type in EventBridge rules\. For information, see [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md) and [Reference: Amazon EventBridge event patterns and types for Systems Manager](reference-eventbridge-events.md)\.

**Topics**
+ [Parameter types and examples](parameter-store-about-examples.md)
+ [SecureString parameters](sysman-paramstore-securestring.md)
+ [Native parameter support for Amazon Machine Image IDs](parameter-store-ec2-aliases.md)
+ [Public parameters](parameter-store-public-parameters.md)
+ [Getting started with Parameter Store](sysman-paramstore-settingup.md)
+ [Working with parameters](sysman-paramstore-working.md)
+ [Parameter Store walkthroughs](sysman-paramstore-walk.md)