# AWS AppConfig<a name="appconfig"></a>

Use AWS AppConfig, a capability of AWS Systems Manager, to create, manage, and quickly deploy application configurations\. AppConfig supports controlled deployments to applications of any size and includes built\-in validation checks and monitoring\. You can use AppConfig with applications hosted on Amazon EC2 instances, AWS Lambda, containers, mobile applications, or IoT devices\.

To prevent errors when deploying application configurations, especially for production systems where a simple typo could cause an unexpected outage, AppConfig includes validators\. A validator provides a syntactic or semantic check to ensure that the configuration you want to deploy works as intended\. To validate your application configuration data, you provide a schema or Lambda function that runs against the configuration\. The configuration deployment or update can only proceed when the configuration data is valid\.

During a configuration deployment, AppConfig monitors the application to ensure that the deployment is successful\. If the system encounters an error, AppConfig rolls back the change to minimize impact for your application users\. You can configure a deployment strategy for each application or environment that includes deployment criteria, including velocity, bake time, and alarms to monitor\. Similar to error monitoring, if a deployment triggers an alarm, AppConfig automatically rolls back to the previous version\. 

AppConfig supports multiple use cases\. Here are some examples\.
+ **Application tuning**: Use AppConfig to carefully introduce changes to your application that can only be tested with production traffic\.
+ **Feature toggle**: Use AppConfig to turn on new features that require a timely deployment, such as a product launch or announcement\. 
+ **User membership**: Use AppConfig to allow premium subscribers to access paid content\. 
+ **Operational issues**: Use AppConfig to reduce stress on your application when a dependency or other external factor impacts the system\.

## How Can AppConfig Benefit My Organization?<a name="learn-more-appconfig-benefits"></a>

AppConfig offers the following benefits\.
+ **Deploy changes across a set of targets quickly**

  AppConfig simplifies the administration of applications at scale by deploying configuration changes from a central location\. AppConfig supports configurations stored in Systems Manager Parameter Store, Systems Manager \(SSM\) documents, and Amazon S3\. You can use AppConfig with applications hosted on Amazon EC2 instances, AWS Lambda, containers, mobile applications, or IoT devices\.
+ **Reduce errors in configuration changes**

  AppConfig reduces application downtime by enabling you to create rules to validate your configuration\. Configurations that aren't valid can't be deployed\. AppConfig provides two options for validating configurations\.
  + For syntactic validation, you can use a JSON schema\. AppConfig validates your configuration by using the JSON schema to ensure that configuration changes adhere to the application requirements\. 
  + For semantic validation, you can call an AWS Lambda function that runs your configuration before you deploy it\.
+ **Update applications without interruptions**

  AppConfig deploys configuration changes to your targets at runtime without a heavy\-weight build process or taking your targets out of service\. 
+ **Control deployment of changes across your application**

  When deploying configuration changes to your targets, AppConfig enables you to minimize risk by using a deployment strategy\. You can use the rate controls of a deployment strategy to determine how fast you want your application targets to receive a configuration change\.

## **What Types of Targets are Supported?**<a name="learn-more-appconfig-operating-systems"></a>

You can use AppConfig with applications hosted on Amazon EC2 instances, AWS Lambda, containers, mobile applications, or IoT devices\. Targets don't need to be configured with the Systems Manager SSM Agent or the AWS Identity and Access Management \(IAM\) instance profile required by other Systems Manager capabilities\. This means that AppConfig works with unmanaged instances\. 

## Is There a Charge to Use AppConfig?<a name="learn-more-appconfig-cost"></a>

Yes\. For more information, see [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\.

## Do I Have to Change My Application to Work with AppConfig?<a name="learn-more-appconfig-code-changes"></a>

Yes\. You must configure your application to poll for new configuration updates by using the [GetConfiguration](http://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetConfiguration.html) API action\. When a new or updated configuration is ready, AppConfig deploys the configuration file to each target in your deployment strategy\.

## How Does AppConfig Work?<a name="learn-more-appconfig-how-it-works"></a>

At a high level, there are three processes for working with AppConfig\.

1. Configure AppConfig to work with your application\.

1. Enable your application code to periodically check for and receive configuration data from AppConfig\.

1. Deploy a new or updated configuration\.

### Configure AppConfig to work with your application<a name="learn-more-appconfig-how-it-works-details-1"></a>

To configure AppConfig to work with your application, you set up three types of resources\.


****  

| Resource | Details | 
| --- | --- | 
|  Application  |  An application in AppConfig is a logical unit of code that provides capabilities for your customers\. For example, an application can be a microservice that runs on Amazon EC2 instances, a mobile application installed by your users, a serverless application using Amazon API Gateway and AWS Lambda, or any system you run on behalf of others\.  | 
|  Environment  |  For each application, you define one or more environments\. An environment is a logical deployment group of AppConfig applications, such as applications in a `Beta` or `Production` environment\. You can also define environments for application subcomponents such as the `Web`, `Mobile`, and `Back-end` components for your application\. You can configure Amazon CloudWatch alarms for each environment\. The system monitors alarms during a configuration deployment\. If an alarm is triggered, the system rolls back the configuration\.  | 
|  Configuration profile  |  A configuration profile includes source information for accessing your configuration data in a Systems Manager \(SSM\) document, a Parameter Store parameter, or in Amazon S3\. A configuration profile can also include optional validators to ensure your configuration data is syntactically and semantically correct\. AppConfig performs a check using the validators when you start a deployment\. If any errors are detected, the deployment stops before making any changes to the targets of the configuration\.  | 

### Enable your application code to periodically check for and receive configuration data from AppConfig<a name="learn-more-appconfig-how-it-works-details-2"></a>

Using the [GetConfiguration](http://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetConfiguration.html) API action, AppConfig offers dynamic configuration updates in a controlled manner\. You must configure your application code to call this API action on a periodic basis\. When called, your code sends the following information: 
+ The IDs of an AppConfig application, environment, and configuration profile\.
+ A unique application instance identifier called a client ID\.
+ The last configuration version known by your application code\.

When a new configuration is deployed \(which means there is a new version of the configuration\), AppConfig responds to the [GetConfiguration](http://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetConfiguration.html) request and returns the new configuration data\.

### Deploy a new or updated configuration<a name="learn-more-appconfig-how-it-works-details-3"></a>

AppConfig enables you to deploy configurations in the manner that best suits the use case of your applications\. You can deploy changes in seconds or you can roll them out slowly to assess the impact of the changes\. The AppConfig resource that helps you control deployments is called a *deployment strategy*\. A deployment strategy includes the following information:
+ Total amount of time for a deployment to last\. \([DeploymentDurationInMinutes](http://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_DeploymentStrategy.html)\)\.
+ The percentage of targets to receive a deployed configuration during each interval\. \([GrowthFactor](http://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_DeploymentStrategy.html)\)\.
+ The amount of time AppConfig monitors for alarms before considering the deployment to be complete and no longer eligible for automatic rollback\. \([FinalBakeTimeInMinutes](http://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_DeploymentStrategy.html)\)\.

You can use built\-in deployment strategies that cover common scenarios or you can create your own\. After you create or choose a deployment strategy, you start the deployment\. Starting the deployment calls the [StartDeployment](http://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_StartDeployment.html) API action\. The call includes the IDs of an application, environment, configuration profile, and \(optionally\) the configuration data version to deploy\. The call also includes the ID of the deployment strategy to use, which determines how the configuration data rolls out\.

## What are the Service Quotas of AppConfig?<a name="learn-more-service-quotas"></a>


****  

| Resource | Quota | 
| --- | --- | 
|  AppConfig applications  |  100  | 
|  Deployment strategies  |  20  | 
|  Environments per AppConfig application  |  20  | 
|  Configuration profiles per AppConfig application  |  100  | 

For information about getting started with and using AppConfig, see the following topics\.

**Topics**
+ [How Can AppConfig Benefit My Organization?](#learn-more-appconfig-benefits)
+ [**What Types of Targets are Supported?**](#learn-more-appconfig-operating-systems)
+ [Is There a Charge to Use AppConfig?](#learn-more-appconfig-cost)
+ [Do I Have to Change My Application to Work with AppConfig?](#learn-more-appconfig-code-changes)
+ [How Does AppConfig Work?](#learn-more-appconfig-how-it-works)
+ [What are the Service Quotas of AppConfig?](#learn-more-service-quotas)
+ [Getting Started with AWS AppConfig](appconfig-getting-started.md)
+ [Working with AWS AppConfig](appconfig-working.md)