# AWS Systems Manager Parameter Store<a name="systems-manager-parameter-store"></a>

AWS Systems Manager Parameter Store provides secure, hierarchical storage for configuration data management and secrets management\. You can store data such as passwords, database strings, and license codes as parameter values\. You can store values as plain text or encrypted data\. You can then reference values by using the unique name that you specified when you created the parameter\. Highly scalable, available, and durable, Parameter Store is backed by the AWS Cloud\. 

Parameter Store offers the following benefits and features\.
+ Use a secure, scalable, hosted secrets management service \(No servers to manage\)\.
+ Improve your security posture by separating your data from your code\.
+ Store configuration data and secure strings in hierarchies and track versions\.
+ Control and audit access at granular levels\.
+ Configure change notifications and trigger automated actions\.
+ Tag parameters individually, and then secure access from different levels, including operational, parameter, Amazon EC2 tag, or path levels\. 
+ Reference AWS Secrets Manager secrets by using Parameter Store parameters\.
+ Use Parameter Store parameters with other Systems Manager capabilities and AWS services to retrieve secrets and configuration data from a central store\. The following AWS services support Parameter Store parameters: Amazon EC2, Amazon Elastic Container Service, AWS Lambda, AWS CloudFormation, AWS CodeBuild, and AWS CodeDeploy\. 
+ Configure integration with AWS Key Management Service \(KMS\), Amazon Simple Notification Service \(SNS\), Amazon CloudWatch, and AWS CloudTrail for encryption, notification, monitoring, and audit capabilities\.

## Getting Started with Parameter Store<a name="sysman-paramstore-gs"></a>

To get started with Systems Manager Parameters, complete the following tasks\.


****  

| Task | For More Information | 
| --- | --- | 
|  Learn about different types of Systems Manager parameters\.  |  [Learn More About Parameters](sysman-paramstore-about.md)  | 
|  Configure parameter access and notifications\.  |  [Setting Up Parameter Store](sysman-paramstore-settingup.md)  | 
|  Learn how to organize, create, and tag parameters\.  |  [Working with Parameters](sysman-paramstore-working.md)  | 
|  Learn about creating and using Systems Manager parameters in a test environment\.  |  [Parameter Store Walkthroughs](sysman-paramstore-walk.md)  | 
|  Learn how Parameter Store uses AWS Key Management Service \(KMS\) to manage secure string parameters\.  |  [How AWS Systems Manager Parameter Store Uses AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/services-parameter-store.html)  | 
|  Learn about the benefits of the advanced\-parameter tier\.  |  [About Advanced Parameters](parameter-store-advanced-parameters.md)  | 
|  Learn how to increase the number of transactions per second that Parameter Store can process\.  |  [Increasing Parameter Store Throughput](parameter-store-throughput.md)  | 

**Related Content**  
The following blog posts provide additional information about Parameter Store and how to use this capability with other AWS services\.
+ For information about Parameter Store limits, see [AWS Systems Manager Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_ssm) in the *Amazon Web Services General Reference*\.
+ [Referencing AWS Secrets Manager Secrets from Parameter Store Parameters](integration-ps-secretsmanager.md)
+ [Managing Secrets for Amazon ECS Applications Using Parameter Store and IAM Roles for Tasks](https://aws.amazon.com/blogs/compute/managing-secrets-for-amazon-ecs-applications-using-parameter-store-and-iam-roles-for-tasks/)
+ [Use Parameter Store to Securely Access Secrets and Config Data in AWS CodeDeploy](https://aws.amazon.com/blogs/mt/use-parameter-store-to-securely-access-secrets-and-config-data-in-aws-codedeploy/)
+ [Interesting Articles on EC2 Systems Manager Parameter Store](https://aws.amazon.com/blogs/mt/interesting-articles-on-ec2-systems-manager-parameter-store/)