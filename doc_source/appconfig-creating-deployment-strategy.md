# Step 4: Create a Deployment Strategy<a name="appconfig-creating-deployment-strategy"></a>

An AWS AppConfig deployment strategy defines the following important aspects of a configuration deployment\.


****  

| Setting | Description | 
| --- | --- | 
|  Deployment type  | Deployment type defines how the configuration deploys or *rolls out*\. AppConfig supports **Linear** deployment types\. This means that AppConfig processes the deployment by dividing the total number of targets by the value specified for **Step percentage**\. For example, a linear deployment that uses a **Step percentage** of 10 deploys the configuration to 10 percent of the hosts\. After those deployments are complete, the system deploys the configuration to the next 10 percent\. This continues until 100% of the targets have successfully received the configuration\.  | 
|  Step percentage  |  This setting specifies the percentage of callers to target during each step of the deployment\.  | 
|  Deployment time  |  This setting specifies an amount of time during which AppConfig deploys to hosts\. This is not a timeout value\. It is a window of time during which the deployment is processed in intervals\.  | 
|  Bake time  |  This setting specifies the amount of time AppConfig monitors for Amazon CloudWatch alarms before proceeding to the next step of a deployment or before considering the deployment to be complete\. If an alarm is triggered during this time, AppConfig rolls back the deployment\. You must configure permissions for AppConfig to roll back based on CloudWatch alarms\. For more information, see [\(Optional\) Configuring Permissions for Rollback Based on CloudWatch Alarms](appconfig-getting-started-cloudwatch-alarms-permissions.md)\.  | 

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

AppConfig creates the deployment strategy\. Proceed to [Step 5: Deploy a Configuration](appconfig-deploying.md)\.