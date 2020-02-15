# Step 2: Create an Environment<a name="appconfig-creating-environment"></a>

For each AWS AppConfig application, you define one or more environments\. An environment is a logical deployment group of AppConfig targets, such as applications in a `Beta` or `Production` environment\. You can also define environments for application subcomponents such as the `Web`, `Mobile`, and `Back-end` components for your application\. You can configure Amazon CloudWatch alarms for each environment\. The system monitors alarms during a configuration deployment\. If an alarm is triggered, the system rolls back the configuration\. 

**Before You Begin**  
If you want to enable AppConfig to roll back a configuration in response to a CloudWatch alarm, then you must configure an AWS Identity and Access Management \(IAM\) role with permissions to enable AppConfig to respond to CloudWatch alarms\. You choose this role in the following procedure\. For more information, see [\(Optional\) Configuring Permissions for Rollback Based on CloudWatch Alarms](appconfig-getting-started-cloudwatch-alarms-permissions.md)\.

Use the following procedure to create an AppConfig environment by using the AWS Systems Manager console\.

**To create an environment**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane choose **AppConfig**\.

1. On the **Applications** tab, choose the application you created in [Step 1: Create an AppConfig Application](appconfig-creating-application.md) and then choose **View details**\.

1. On the **Environments** tab, choose **Create environment**\.

1. For **Name**, enter a name for the environment\.

1. For **Description**, enter information about the environment\.

1. In the **Monitors** section, choose **Enable rollback on CloudWatch Alarms** if you want AppConfig to roll back a configuration when an alarm is triggered\.

1. In the **IAM role** list, choose the IAM role with permission to roll back a configuration when an alarm is triggered\.

1. In the **CloudWatch alarms** list, choose one or more alarms to monitor\.

1. In the **Tags** section, enter a key and an optional value\. You can specify a maximum of 50 tags for a resource\. 

1. Choose **Create environment**\.

AppConfig creates the environment and then displays the **Environment details** page\. Proceed to [Step 3: Create a Configuration and a Configuration Profile](appconfig-creating-configuration-and-profile.md)\.