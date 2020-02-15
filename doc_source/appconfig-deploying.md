# Step 5: Deploy a Configuration<a name="appconfig-deploying"></a>

Starting a deployment in AWS AppConfig calls the [StartDeployment](http://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_StartDeployment.html) API action\. This call includes the IDs of the AppConfig application, the environment, the configuration profile, and \(optionally\) the configuration data version to deploy\. The call also includes the ID of the deployment strategy to use, which determines how the configuration data is deployed\.

AppConfig monitors the distribution to all hosts and reports status\. If a distribution fails, then AppConfig rolls back the configuration\.

Use the following procedure to deploy an AppConfig configuration by using the AWS Systems Manager console\.

**To deploy a configuration**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **AppConfig**\.

1. On the **Applications** tab, choose an application, and then choose **View details**\.

1. On the **Environments** tab, choose an environment, and then choose **View details**\.

1. Choose **Start deployment**\.

1. For **Configuration**, choose a configuration from the list\.

1. Depending on the source of your configuration, use the **Document version** or **Parameter version** list to choose the version you want to deploy\. 

1. For **Deployment strategy**, choose a strategy from the list\. 

1. For **Deployment description**, enter a description\. 

1. In the **Tags** section, enter a key and an optional value\. You can specify a maximum of 50 tags for a resource\. 

1. Choose **Start deployment**\.