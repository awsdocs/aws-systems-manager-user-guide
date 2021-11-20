# AWS Systems Manager Quick Setup<a name="systems-manager-quick-setup"></a>

Use Quick Setup, a capability of AWS Systems Manager, to quickly configure frequently used Amazon Web Services services and features with recommended best practices\. Quick Setup simplifies setting up services, including Systems Manager, by automating common or recommended tasks\. These tasks include, for example, creating required AWS Identity and Access Management \(IAM\) instance profile roles and setting up operational best practices, such as periodic patch scans and inventory collection\. 

## What are the benefits of Quick Setup?<a name="quick-setup-features"></a>

Benefits of Quick Setup include the following:
+ **Simplify service and feature configuration**

  Quick Setup walks you through configuring operational best practices and automatically deploys those configurations\. The Quick Setup dashboard displays a real\-time view of your configuration deployment status\. 
+ **Deploy configurations automatically across multiple accounts**

  You can use Quick Setup in an individual AWS account or across multiple AWS accounts and AWS Regions by integrating with AWS Organizations\. Using Quick Setup across multiple accounts helps to ensure that your organization maintains consistent configurations\.
+ **Eliminate configuration drift**

  Configuration drift occurs whenever a user makes any change to a service or feature that conflicts with the selections made through Quick Setup\. Quick Setup periodically checks for configuration drift and attempts to remediate it\.

## Who should use Quick Setup?<a name="quick-setup-audience"></a>

Quick Setup is most beneficial for customers who already have some experience with the services and features they're setting up, and want to simplify their setup process\. If you're unfamiliar with the AWS service you're configuring with Quick Setup, we recommend that you learn more about the service\. Review the content in the relevant User Guide before you create a configuration with Quick Setup\.

## How to access Quick Setup<a name="quick-setup-access"></a>

To access this capability, choose **Quick Setup** in the navigation pane of the Systems Manager console\. To access the **Organization Quick Setup** type, which you can use to target multiple accounts and Regions, sign in to the management account for your organization before accessing Quick Setup\. For more information, see [Getting started with AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_getting-started.html) in the *AWS Organizations User Guide*\. 