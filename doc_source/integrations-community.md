# Integration examples from the community<a name="integrations-community"></a>

The following sections provide links to blog posts, articles, and community\-provided examples\.

**Note**  
These links are provided for informational purposes only, and should not be considered either a comprehensive list or an endorsement of the content of the examples\. AWS is not responsible for the content or accuracy of this content\. 

## Blog posts<a name="integrations-community-blogposts"></a>

**Application Management**
+ [A complete guide to using the AWS Systems Manager Parameter Store](https://seanjziegler.com/a-complete-guide-to-using-the-aws-systems-manager-parameter-store/)

  Sean Ziegler offers a concise overview of Parameter Store functionality and provides a Boto3 example for interacting with Parameter Store\.

  *Published May 2020*
+ [Keep Your Secrets Safe with AWS Systems Manager Parameter Store and Node](https://www.codebyamir.com/blog/keep-your-secrets-safe-with-aws-systems-manager-parameter-store-and-nodejs)

  Amir Boroumand walks through how to save and retrieve a password using the Parameter Store with Node\.

  *Published October 2019*
+ [Using SSM Parameters with CloudFormation Templates and Terraform Projects](https://start.jcolemorrison.com/using-ssm-parameters-with-cloudformation-templates-and-terraform-projects/)

  J Cole Morrison demonstrates how to create parameters for use in AWS CloudFormation templates and Terraform projects so you can reference values with less human error\.

  *Published August 2019*
+ [Using AWS Systems Manager Parameter Store for Configuration](https://github.com/aws-samples/aws-net-guides/tree/master/Communications/ParameterStore-Example)

  Learn how to store and retrieve application configuration settings at runtime for an application instead of hard\-coding the configuration values into a sample application's code and configuration files\. The sample application, a simple \.NET Core console application, shows how to use the AWS SDK for \.NET to retrieve configuration values from Parameter Store\.

  *Published March 2019*
+ [Using AWS Systems Manager Parameter Store SecureString parameters in AWS CloudFormation templates](http://aws.amazon.com/blogs/mt/using-aws-systems-manager-parameter-store-secure-string-parameters-in-aws-cloudformation-templates/)

  Learn how to use plaintext and `SecureString` parameters in your AWS CloudFormation templates through the use of dynamic references to fetch parameter values\.

  *Published October 2018*
+ [Use parameter labels for easy configuration update across environments](http://aws.amazon.com/blogs/mt/use-parameter-labels-for-easy-configuration-update-across-environments/)

  Learn how to use a parameter label as an alias for a parameter version\. You can group parameter versions across hierarchies using labels, and you can deploy new updates to parameters across environments using hierarchies and labels\.

  *Published July 2018*
+ [Integrating AWS CloudFormation with AWS Systems Manager Parameter Store](http://aws.amazon.com/blogs/mt/integrating-aws-cloudformation-with-aws-systems-manager-parameter-store/)

  Learn how to use Parameter Store parameters in your AWS CloudFormation templates to simplify stack updates involving parameters and achieve consistency by using values stored in Parameter Store\. With this integration, your code remains untouched while the stack update operation automatically picks up the latest parameter value\.

  *Published December 2017*
+ [The Right Way to Store Secrets using Parameter Store](http://aws.amazon.com/blogs/mt/the-right-way-to-store-secrets-using-parameter-store/)

  Evan Johnson presents a case study in how Segment centrally and securely manages their secrets using a combination of Parameter Store, lots of Terraform code, and chamber, with a focus on getting up and running with Parameter Store in production\.

  *Published August 2017*
+ [Join a Microsoft Active Directory Domain with Parameter Store and Amazon EC2 Systems Manager Documents](http://aws.amazon.com/blogs/mt/join-a-microsoft-active-directory-domain-with-parameter-store-and-amazon-ec2-systems-manager-documents/)

  Read about a scenario for centralized configuration management, with an example of joining EC2 instances to a Microsoft Active Directory\. This post shows you how to launch an EC2 instance that consumes and uses configuration values stored as Parameter Store parameters to join your Active Directory domain\.

  *Published June 2017*
+  [Use Parameter Store to securely access secrets and config data in AWS CodeDeploy](http://aws.amazon.com/blogs/mt/use-parameter-store-to-securely-access-secrets-and-config-data-in-aws-codedeploy/) 

  Learn how to simplify your CodeDeploy workflows by using Parameter Store to store and reference a configuration secret\. Doing so not only improves your security posture, but also automates your deployment because you don’t have to manually change configuration data in your source code\.

  *Published March 2017*
+ [Secrets in AWS](https://stp5.net/blog/post/secrets-in-aws/)

  Stephen Price describes how you can use Parameter Store to manage and use secrets in your favorite programming language to handle secrets for cloud\-based architectures, such as microservices or containerized applications\.

  *Published March 2017*
+ [Using Parameter Store with AWS CodePipeline](https://stelligent.com/2017/03/09/using-parameter-store-with-aws-codepipeline/)

  Trey McElhattan demonstrates how to effectively use Parameter Store as part of a continuous delivery pipeline using AWS CodePipeline\. 

  *Published March 2017*
+ [Managing Secrets for Amazon ECS Applications Using Parameter Store and IAM Roles for Tasks](http://aws.amazon.com/blogs/compute/managing-secrets-for-amazon-ecs-applications-using-parameter-store-and-iam-roles-for-tasks/)

  By using Parameter Store and task IAM roles, you can create a central secret management store and a well integrated access layer that allows applications to access only the keys they need, to restrict access on a container basis, and to further encrypt secrets with custom KMS keys\.

  *Published January 2017*

**Actions & Change**
+  [Providing temporary instance permissions with AWS Systems Manager Automations](http://aws.amazon.com/blogs/mt/providing-temporary-instance-permissions-with-aws-systems-manager-automations/) 

  Learn how to provide temporary permissions to Amazon EC2 instances within your Automation documents\. This helps you to avoid modifying instance profiles that are attached to instances long term and only contain the core permissions required for Systems Manager functionality\.

  *Published December 2019*
+ [Automating the Cloud: AWS Security Done Efficiently](https://blog.rapid7.com/2019/08/19/automating-the-cloud-aws-security-done-efficiently/)

  Josh Frantz, Lead Security Consultant at Rapid7, shows how to automate common tasks so you can more efficiently secure your AWS environment and focus on solving important, engaging, and difficult issues\.

  *Published August 2019*
+  [Managing AWS resources across multiple accounts and Regions using AWS Systems Manager Automation](http://aws.amazon.com/blogs/mt/managing-aws-resources-across-multiple-accounts-and-regions-using-aws-systems-manager-automation/) 

  Learn how to manage your resources across multiple AWS accounts and Regions using Systems Manager Automation\.

  *Published January 2019*
+ [Onica Demonstrates Uses for New AWS Systems Manager Automation Actions](https://onica.com/blog/devops/aws-blog-on-demonstrating-uses-for-new-aws-systems-manager-automation-by-onica/)

  Onica uses real world examples representing some of the problems they see in the field while assisting AWS customers to demonstrate how Automation actions can be used to solve these problems\.

  *Published August 2018*

**Instances & Nodes**
+  [How to patch Amazon EC2 Windows instances in private subnets using AWS Systems Manager](http://aws.amazon.com/blogs/mt/how-to-patch-windows-ec2-instances-in-private-subnets-using-aws-systems-manager/) 

  Learn how to patch Amazon EC2 Windows instances in private subnets without internet connectivity\.

  *Published December 2018*
+  [Centralized multi\-account and multi\-Region patching with AWS Systems Manager Automation](http://aws.amazon.com/blogs/mt/centralized-multi-account-and-multi-region-patching-with-aws-systems-manager-automation/) 

  Learn how to use Systems Manager Automation to patch your managed instances across multiple AWS accounts and Regions\.

  *Published November 2018*
+  [Scalable cross\-platform patching with AWS Systems Manager](http://aws.amazon.com/blogs/mt/scalable-cross-platform-patching-with-aws-systems-manager/) 

  Learn how to patch Amazon EC2 instances at scale with Systems Manager\.

  *Published April 2018*

**Shared Resources**
+  [Writing your own AWS Systems Manager documents](http://aws.amazon.com/blogs/mt/writing-your-own-aws-systems-manager-documents/) 

  Learn how to author your own Systems Manager documents\.

  *Published May 2018*