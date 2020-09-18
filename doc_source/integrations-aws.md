# Integration with AWS services<a name="integrations-aws"></a>

Through the use of `Command` and `Automation` type SSM documents, you can use Systems Manager to integrate with many AWS services\. For more information about these SSM documents, see [AWS Systems Manager documents](sysman-ssm-docs.md)\.

Systems Manager is integrated with the following AWS services\.

## Compute<a name="integrations-aws-compute"></a>


|  |  | 
| --- |--- |
| Amazon Elastic Compute Cloud \(Amazon EC2\) |  [Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/) provides scalable computing capacity in the Amazon Web Services \(AWS\) Cloud\. Using Amazon EC2 eliminates your need to invest in hardware up front, so you can develop and deploy applications faster\. You can use Amazon EC2 to launch as many or as few virtual servers as you need, configure security and networking, and manage storage\. Systems Manager enables you to perform several tasks on Amazon EC2 instances\. For example you can launch, configure, manage, maintain, troubleshoot, and securely connect to your Amazon EC2 instances\. You can also use Systems Manager to deploy software, evaluate compliance, and gather inventory from your Amazon EC2 instances\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/integrations-aws.html)  | 
| Amazon EC2 Auto Scaling |  [Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/) helps you ensure that you have the correct number of Amazon EC2 instances available to handle the load for your application\. You create collections of EC2 instances, called Auto Scaling groups\. Systems Manager enables you to automate common procedures like patching the Amazon Machine Image \(AMI\) used in your Auto Scaling template for your Auto Scaling group\.  Learn more [Walkthrough: Patch an AMI and update an Auto Scaling group](automation-walk-patch-windows-ami-autoscaling.md)   | 
| Amazon Elastic Container Service \(Amazon ECS\) |  [Amazon ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/) is a highly scalable, fast, container management service that makes it easy to run, stop, and manage Docker containers on a cluster\. Systems Manager enables you to manage container instances remotely and inject sensitive data into your containers by storing your sensitive data in AWS Systems Manager Parameter Store parameters and then referencing them in your container definition\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/integrations-aws.html)  | 
| AWS Lambda |  [Lambda](https://docs.aws.amazon.com/lambda/latest/dg/) is a compute service that lets you run code without provisioning or managing servers\. AWS Lambda runs your code only when needed and scales automatically, from a few requests per day to thousands per second\. Systems Manager enables you to use Lambda functions within Automation document content by using the `aws:invokeLambdaFunction` action\.  Learn more [Walkthrough: Simplify AMI patching using Automation, AWS Lambda, and Parameter Store](automation-walk-patch-windows-ami-simplify.md)   | 

## Storage<a name="integrations-aws-productgroup"></a>


|  |  | 
| --- |--- |
| Amazon Simple Storage Service \(Amazon S3\) |  [Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/) is storage for the Internet\. It is designed to make web\-scale computing easier for developers\. Amazon S3 has a simple web services interface that you can use to store and retrieve any amount of data, at any time, from anywhere on the web\. Systems Manager enables you to run remote scripts and Systems Manager documents that are stored in Amazon S3\. Distributor uses Amazon S3 to store packages\. You can also send output for Run Command and Session Manager to Amazon S3\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/integrations-aws.html)  | 

## Developer Tools<a name="integrations-aws-developer-tools"></a>


|  |  | 
| --- |--- |
| AWS CodeBuild |  [CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/) is a fully managed build service in the cloud\. CodeBuild compiles your source code, runs unit tests, and produces artifacts that are ready to deploy\. CodeBuild eliminates the need to provision, manage, and scale your own build servers\. Parameter Store enables you to store sensitive information for your build specifications and projects\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/integrations-aws.html)  | 

## Security, Identity, and Compliance<a name="integrations-aws-security-identify-compliance"></a>


|  |  | 
| --- |--- |
| AWS Identity and Access Management \(IAM\) |  [IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/) is a web service that helps you securely control access to AWS resources\. You use IAM to control who is authenticated \(signed in\) and authorized \(has permissions\) to use resources\. Systems Manager enables you to control access to services using IAM\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/integrations-aws.html)  | 
|  AWS Secrets Manager  |  [Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/) provides easier management of secrets\. Secrets can be database credentials, passwords, third\-party API keys, and even arbitrary text\. Parameter Store enables you to retrieve Secrets Manager secrets when using other AWS services that already support references to Parameter Store parameters\.  Learn more [Referencing AWS Secrets Manager secrets from Parameter Store parameters](integration-ps-secretsmanager.md)   | 

## Cryptography and PKI<a name="integrations-aws-cryptography-pki"></a>


|  |  | 
| --- |--- |
| AWS Key Management Service \(AWS KMS\) |  [AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/) is a managed service that makes it easy for you to create and control customer master keys \(CMKs\), the encryption keys used to encrypt your data\. Systems Manager enables you to use AWS KMS to create `SecureString` parameters and encrypt Session Manager session data\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/integrations-aws.html)  | 

## Management and Governance<a name="integrations-aws-management-governance"></a>


|  |  | 
| --- |--- |
| AWS CloudFormation |  [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/) is a service that helps you model and set up your Amazon Web Services resources so that you can spend less time managing those resources and more time focusing on your applications that run in AWS\. Parameter Store is a source for dynamic references\. Dynamic references provide a compact, powerful way for you to specify external values that are stored and managed in other services in your AWS CloudFormation stack templates\.  Learn more [Using dynamic references to specify template values](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/dynamic-references.html)   | 
| AWS CloudTrail |  [CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/) is an AWS service that helps you enable governance, compliance, and operational and risk auditing of your AWS account\. Actions taken by a user, role, or an AWS service are recorded as events in CloudTrail\. Events include actions taken in the AWS Management Console, AWS Command Line Interface, and AWS SDKs and APIs\. Systems Manager integrates with CloudTrail, enabling you to capture all API calls for Systems Manager as events, including calls from the Systems Manager console and from code calls to the Systems Manager APIs\.  Learn more [Logging AWS Systems Manager API calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)   | 
| Amazon CloudWatch Logs |  [Amazon CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/) enables you to centralize the logs from all of your systems, applications, and AWS services that you use\. You can then easily view them, search them for specific error codes or patterns, filter them based on specific fields, or archive them securely for future analysis\. Systems Manager supports sending logs for the SSM Agent, Run Command, and Session Manager to CloudWatch Logs\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/integrations-aws.html)  | 
| Amazon EventBridge |  [EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/) delivers a near real\-time stream of system events that describes changes in Amazon Web Services \(AWS\) resources\. Using simple rules that you can quickly set up, you can match events and route them to one or more target functions or streams\. EventBridge becomes aware of operational changes as they occur\. EventBridge responds to these operational changes and takes corrective action as necessary\. These actions include sending messages to respond to the environment, activating functions, and capturing state information\. Systems Manager has multiple events that are supported by EventBridge enabling you to take actions based on the content of those events\.  Learn more [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md) Amazon EventBridge is the preferred way to manage your events\. CloudWatch Events and EventBridge are the same underlying service and API, but EventBridge provides more features\. Changes you make in either CloudWatch or EventBridge will appear in each console\. For more information, see the [https://docs.aws.amazon.com/eventbridge/latest/userguide/](https://docs.aws.amazon.com/eventbridge/latest/userguide/)\.  | 
| AWS Config |  [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/) provides a detailed view of the configuration of AWS resources in your AWS account\. This includes how the resources are related to one another and how they were configured\. This enables you to see how the configurations and relationships change over time\. Systems Manager is integrated with AWS Config, providing multiple rules that help you gain visibility into your Amazon EC2 instances\. These rules help you identify which Amazon EC2 instances are managed by Systems Manager, operating system configurations, system\-level updates, installed applications, network configurations, and more\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/integrations-aws.html)  | 
| AWS Trusted Advisor |  [Trusted Advisor](https://aws.amazon.com/premiumsupport/trustedadvisor/) is an online tool that provides real\-time guidance to help you provision your resources following AWS best practices\. Systems Manager hosts Trusted Advisor and you can view Trusted Advisor data in Explorer\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/integrations-aws.html)  | 

## Networking and Content Delivery<a name="integrations-aws-networking-content-delivery"></a>


|  |  | 
| --- |--- |
| AWS PrivateLink |  [AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-services-overview.html) enables you to privately connect your VPC to supported AWS services and VPC endpoint services without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection\. Systems Manager supports managed instances connecting to Systems Manager APIs using AWS PrivateLink\. This improves the security posture of your managed instances since AWS PrivateLink restricts all network traffic between your managed instances, Systems Manager, and Amazon EC2 to the Amazon network\. This means that managed instances are not required to have access to the internet\.  Learn more [\(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md)    | 

## Analytics<a name="integrations-aws-analytics"></a>


|  |  | 
| --- |--- |
| Amazon Athena |  [Athena](https://docs.aws.amazon.com/athena/latest/ug/) is an interactive query service that makes it easy to analyze data directly in Amazon Simple Storage Service \(Amazon S3\) using standard SQL\. With a few actions in the AWS Management Console, you can point Athena at your data stored in Amazon S3 and begin using standard SQL to run ad hoc queries and get results in seconds\. Systems Manager Inventory integrates with Athena to help you query inventory data from multiple AWS Regions and accounts\. Athena integration uses resource data sync so that you can view inventory data from all of your managed instances on the **Detailed View** page in the Systems Manager Inventory console\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/integrations-aws.html)  | 
| AWS Glue |  [AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/) is a fully managed ETL \(extract, transform, and load\) service that makes it simple and cost\-effective to categorize your data, clean it, enrich it, and move it reliably between various data stores and data streams\.  Systems Manager uses AWS Glue to crawl the Inventory data in your Amazon Simple Storage Service \(Amazon S3\) bucket\.  Learn more [Querying inventory data from multiple Regions and accounts](systems-manager-inventory-query.md)   | 
| Amazon QuickSight |  [Amazon QuickSight](https://docs.aws.amazon.com/quicksight/latest/user/) is a business analytics service you can use to build visualizations, perform ad hoc analysis, and get business insights from your data\. It can automatically discover AWS data sources and also works with your data sources\. Systems Manager resource data sync sends inventory data collected from all of your managed instances to a single Amazon S3 bucket\. You can use Amazon QuickSight to query and analyze the aggregated data\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/integrations-aws.html)  | 

## Application Integration<a name="integrations-aws-application-integration"></a>


|  |  | 
| --- |--- |
| Amazon Simple Notification Service \(Amazon SNS\) |  [Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/)is a web service that coordinates and manages the delivery or sending of messages to subscribing endpoints or clients\. Systems Manager generates statuses for multiple services that can be captured by Amazon SNS notifications\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/integrations-aws.html)  | 

## AWS Management Console<a name="integrations-aws-management-console"></a>


|  |  | 
| --- |--- |
| AWS Resource Groups |  [Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/) organize your AWS resources\. Resource groups make it easier to manage, monitor, and automate tasks on large numbers of resources at one time\. Systems Manager resource types like managed instances, SSM documents, maintenance windows, Parameter Store parameters, and patch baselines can be added to resource groups\.  Learn more [Resource Groups in AWS Systems Manager](systems-manager-resource-groups.md)   | 

**Topics**
+ [Compute](#integrations-aws-compute)
+ [Storage](#integrations-aws-productgroup)
+ [Developer Tools](#integrations-aws-developer-tools)
+ [Security, Identity, and Compliance](#integrations-aws-security-identify-compliance)
+ [Cryptography and PKI](#integrations-aws-cryptography-pki)
+ [Management and Governance](#integrations-aws-management-governance)
+ [Networking and Content Delivery](#integrations-aws-networking-content-delivery)
+ [Analytics](#integrations-aws-analytics)
+ [Application Integration](#integrations-aws-application-integration)
+ [AWS Management Console](#integrations-aws-management-console)
+ [Running scripts from Amazon S3](integration-s3.md)
+ [Referencing AWS Secrets Manager secrets from Parameter Store parameters](integration-ps-secretsmanager.md)