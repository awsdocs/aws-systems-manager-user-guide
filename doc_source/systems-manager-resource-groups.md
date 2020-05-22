# Resource Groups in AWS Systems Manager<a name="systems-manager-resource-groups"></a>

You can use *resource groups* to organize your AWS resources\. Resource groups make it easier to manage, monitor, and automate tasks on large numbers of resources at one time\.

AWS Resource Groups provides two general methods for defining a resource group\. Both methods involve using a query to identify the members for a group\. 

The first method relies on tags applied to AWS resources to add resources to a group\. Using this method, you apply the same key/value pair tags to resources of various types in your account and then use the AWS Resource Groups service to create a group based on that tag pair\. 

The second method is based on resources available in an individual AWS CloudFormation stack\. Using this method, you choose an AWS CloudFormation stack, and then choose resource types in the stack that you want to be in the group\. 

For more information about these methods, see [Build Queries and Groups in AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/gettingstarted-query.html) in the *AWS Resource Groups User Guide*\.

In order to be added to a resource group, the resources must all be in the same AWS Region\. The resources must also be one of the *resource types* supported for use in resource groups\. For example, in the AWS Identity and Access Management \(IAM\) service, you can apply tags to both users and roles, but only roles are supported by resource groups\. In the Amazon Simple Notification Service \(Amazon SNS\), you can add tags to topics to add them to a resource group, but you can't add tags to subscriptions\. In AWS Systems Manager, you can apply tags to several resource types, including maintenance windows, managed instances, and patch baselines, but not to change calendars or Distributor packages\.

You can add tags to your AWS resources in different ways:
+ When you create the resource
+ When you update the resource
+ Using the **Tag Editor** in the Resource Groups service

**Supported AWS resource types**  
For a list of all services with resources types that can be added to resource groups through the use of tags, and the resources they support, see [Supported Resources](url-arg-user;supported-resources.html) in the *AWS Resource Groups User Guide*\.

**Supported Systems Manager resource types**  
The Systems Manager resource types that you can tag in order to add them to resource groups include the following:
+ SSM documents
+ Managed instances
+ Maintenance windows
+ SSM parameters
+ Patch baselines

**Permissions to work with resource groups and Tag Editor**  
Before users in your AWS account can work with resource groups and tags in the Resource Groups service and Tag Editor, a user with administrator access must provide the users with the necessary permissions\. For information about granting the Systems Manager users in your account access to Resource Groups and Tag Editor in the AWS Management Console and the Resource Groups capability in Systems Manager, see [Create user groups](setup-create-users-nonadmin-groups.md)\.

**Using the Tag Editor**  
Using the Tag Editor is the most efficient way to add many resource types to a resource group\. You can view all supported resource types in your account from the same page\. For resources of certain types, choose just the resources you want to add to the resource group, and add the tags to them in bulk\. For information about using the Tag Editor, see [Find Resources to Tag](https://docs.aws.amazon.com/ARG/latest/userguide/find-resources-to-tag.html) and [Manage Tags](https://docs.aws.amazon.com/ARG/latest/userguide/tagging-resources.html) in the *AWS Resource Groups User Guide*\.

**What else can I use tagged resources for?**  
Adding resources to a resource group is just one major use of resource tags\. You can also use tags to specify resources with certain tags applied as the targets of AWS operations\. You can search for resources with the same tags applied to them\. You can craft IAM policies to grant or deny users access to resources that are tagged with those tags\. 

**What can I do with resource groups?**  
Several AWS services, including Systems Manager, let you act on, monitor, or share AWS resources as a group\. For example, in Amazon CloudWatch, you can [focus your view to display metrics and alarms from a single resource group](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Automatic_Dashboards_Resource_Group.html)\. In AWS Resource Access Manager, you can [share the AWS resources you have added to a resource group](https://docs.aws.amazon.com/ram/latest/userguide/shareable.html#shareable-arg), as a group, with other accounts that you choose\. 

For information about all AWS services that can use resource groups, see [Service Integrations with AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/orgs_integrated-services-list.html) in the *AWS Resource Groups User Guide*\.

In Systems Manager, you can work with resource groups in a number of ways\.

First, you can create and manage resource groups\. Systems Manager includes a console that provides the same functionality as the Resource Groups service console\. To access this console, do the following:

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Resource Groups**\.

For information about working with the Resource Groups console, see [Getting Started with AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/gettingstarted.html) in the *AWS Resource Groups User Guide*\.

Second, in the Inventory capability, you can select the managed instances for which you want to view compliance data by specifying a resource group to which they belong\.

Third, you can specify a resource group as the target for the following:
+ A command you run in Run Command\.
+ An Automation workflow you run in Automation\. 
+ An association you create in State Manager\.
+ A maintenance window you create in Maintenance Windows\.
+ A package installation or update operation in Distributor\. 

**Topics**
+ [Viewing operations data for AWS Resource Groups](viewing-operations-data.md)

**Related AWS Blog Posts**  
Refer to the following AWS blog posts for more information about resource groups\.
+ [Use Tag Policies to Manage Tags Across Multiple AWS Accounts](http://aws.amazon.com/blogs/aws/new-use-tag-policies-to-manage-tags-across-multiple-aws-accounts/) \(26 NOV 2019\)
+ [New AWS Resource Access Manager – Cross\-Account Resource Sharing](http://aws.amazon.com/blogs/aws/new-aws-resource-access-manager-cross-account-resource-sharing/) \(21 NOV 2018\)
+ [Simplify granting access to your AWS resources by using tags on AWS IAM users and roles](http://aws.amazon.com/blogs/security/simplify-granting-access-to-your-aws-resources-by-using-tags-on-aws-iam-users-and-roles/) \(19 NOV 2018\)
+ [The Resource Groups Tagging API Makes It Easier to List Your Resources by Using a New Pagination Parameter](http://aws.amazon.com/blogs/security/the-resource-groups-tagging-api-now-supports-pagination-by-the-number-of-resources-and-automated-pagination-in-the-aws-cli/) \(22 MAY 2017\)
+ [Centrally Manage Tags and Search for Resources Across AWS Services by Using the New Resource Groups Tagging API](http://aws.amazon.com/blogs/security/centrally-manage-tags-and-search-for-resources-across-aws-services-by-using-the-new-resource-groups-tagging-api/) \(30 MAR 2017\)
+ [Organize Your AWS Resources by Using up to 50 Tags per Resource](http://aws.amazon.com/blogs/security/now-organize-your-aws-resources-by-using-up-to-50-tags-per-resource/) \(15 AUG 2016, updated 28 DEC 2017\)