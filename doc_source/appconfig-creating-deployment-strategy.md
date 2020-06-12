# Step 4: Creating a deployment strategy<a name="appconfig-creating-deployment-strategy"></a>

An AWS AppConfig deployment strategy defines the following important aspects of a configuration deployment\.


****  

| Setting | Description | 
| --- | --- | 
|  Deployment type  | Deployment type defines how the configuration deploys or *rolls out*\. AppConfig supports **Linear** and **Exponential** deployment types\. [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/appconfig-creating-deployment-strategy.html)  | 
|  Step percentage \(growth factor\)  |  This setting specifies the percentage of callers to target during each step of the deployment\.  In the SDK and the [AppConfig API Reference](http://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_CreateDeploymentStrategy.html), `step percentage` is called `growth factor`\.   | 
|  Deployment time  |  This setting specifies an amount of time during which AppConfig deploys to hosts\. This is not a timeout value\. It is a window of time during which the deployment is processed in intervals\.  | 
|  Bake time  |  This setting specifies the amount of time AppConfig monitors for Amazon CloudWatch alarms before proceeding to the next step of a deployment or before considering the deployment to be complete\. If an alarm is triggered during this time, AppConfig rolls back the deployment\. You must configure permissions for AppConfig to roll back based on CloudWatch alarms\. For more information, see [\(Optional\) Configuring permissions for rollback based on CloudWatch alarms](appconfig-getting-started-cloudwatch-alarms-permissions.md)\.  | 

## Predefined deployment strategies<a name="appconfig-creating-deployment-strategy-predefined"></a>

AppConfig includes predefined deployment strategies to help you quickly deploy a configuration\. Instead of creating your own strategies, you can choose one of the following when you deploy a configuration\.


****  

| Deployment strategy | Description | 
| --- | --- | 
|  AppConfig\.AllAtOnce  | **Quick**: This strategy deploys the configuration to all targets immediately\. The system monitors for Amazon CloudWatch alarms for 10 minutes\. If no alarms are received in this time, the deployment is complete\. If an alarm is triggered during this time, AppConfig rolls back the deployment\.   | 
|  AppConfig\.Linear50PercentEvery30Seconds  | **Testing/Demonstration**: This strategy deploys the configuration to half of all targets every 30 seconds for a one\-minute deployment\. The system monitors for Amazon CloudWatch alarms for 1 minute\. If no alarms are received in this time, the deployment is complete\. If an alarm is triggered during this time, AppConfig rolls back the deployment\.We recommend using this strategy only for testing or demonstration purposes because it has a short duration and bake time\.  | 
|  AppConfig\.Canary10Percent20Minutes  | **AWS Recommended**: This strategy processes the deployment exponentially using a 10% growth factor over 20 minutes\. The system monitors for Amazon CloudWatch alarms for 10 minutes\. If no alarms are received in this time, the deployment is complete\. If an alarm is triggered during this time, AppConfig rolls back the deployment\.We recommend using this strategy for production deployments because it aligns with AWS best practices for configuration deployments\.  | 

## Create a deployment strategy<a name="appconfig-creating-deployment-strategy-create"></a>

You can create a maximum of 20 deployment strategies\. When you deploy a configuration, you can choose the deployment strategy that works best for the application and the environment\.

Use the following procedure to create an AppConfig deployment strategy by using the AWS Systems Manager console\.

**To create a deployment strategy**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **AppConfig**\.

1. Choose the **Deployment Strategies** tab, and then choose **Create deployment strategy**\.

1. For **Name**, enter a name for the deployment strategy\.

1. For **Description**, enter information about the deployment strategy\.

1. For **Deployment type**, choose a type\.

1. For **Step percentage**, choose the percentage of callers to target during each step of the deployment\. 

1. For **Deployment time**, enter the total duration for the deployment in minutes or hours\. 

1. For **Bake time**, enter the total time, in minutes or hours, to monitor for Amazon CloudWatch alarms before proceeding to the next step of a deployment or before considering the deployment to be complete\. 

1. In the **Tags** section, enter a key and an optional value\. You can specify a maximum of 50 tags for a resource\. 

1. Choose **Create deployment strategy**\.

AppConfig creates the deployment strategy\. Proceed to [Step 5: Deploying a configuration](appconfig-deploying.md)\.