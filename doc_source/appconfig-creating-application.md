# Step 1: Create an AppConfig application<a name="appconfig-creating-application"></a>

An application in AWS AppConfig is a logical unit of code that provides capabilities for your customers\. For example, an application can be a microservice that runs on Amazon EC2 instances, a mobile application installed by your users, a serverless application using Amazon API Gateway and AWS Lambda, or any system you run on behalf of others\. 

Use the following procedure to create an AppConfig application by using the AWS Systems Manager console\.

**To create an application**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane choose **AppConfig**\.

1. On the **Applications** tab, choose **Create application**\.

1. For **Name**, enter a name for the application\.

1. For **Description**, enter information about the application\.

1. In the **Tags** section, enter a key and an optional value\. You can specify a maximum of 50 tags for a resource\. 

1. Choose **Create application**\.

AppConfig creates the application and then displays the **Environments** tab\. Proceed to [Step 2: Create an environment](appconfig-creating-environment.md)\. You can begin the procedure where it states, "On the **Environments** tab\.\.\."