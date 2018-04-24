# AWS Systems Manager Automation<a name="systems-manager-automation"></a>

Systems Manager Automation is an AWS\-hosted service that simplifies common instance and system maintenance and deployment tasks\. For example, you can use Automation as part of your change management process to keep your Amazon Machine Images \(AMIs\) up\-to\-date with the latest application build\. Or, let’s say you want to create a backup of a database and upload it nightly to Amazon S3\. With Automation, you can avoid deploying scripts and scheduling logic directly to the instance\. Instead, you can run maintenance activities through Systems Manager Run Command and AWS Lambda steps orchestrated by the Automation service\.

Automation enables you to do the following\.
+ Pre\-install and configure applications and agents in your Amazon Machine Images \(AMIs\) using a streamlined and repeatable process that you can audit\.
+ Build workflows to configure and manage instances and AWS resources\.
+ Create your own custom workflows, or use pre\-defined workflows maintained by AWS\.
+ Receive notifications about Automation tasks and workflows by using Amazon CloudWatch Events
+ Monitor Automation progress and execution details by using the Amazon EC2 or the AWS Systems Manager console\. 

**Topics**
+ [AWS Systems Manager Automation Concepts](automation-concepts.md)
+ [Automation Use Cases](automation-use-cases.md)
+ [Automation QuickStarts](automation-quickstart.md)
+ [Setting Up Automation](automation-setup.md)
+ [Systems Manager Automation Walkthroughs](automation-walk.md)
+ [Working with Automation Documents](automation-documents.md)
+ [Systems Manager Automation Examples](automation-examples.md)
+ [Automation System Variables](automation-variables.md)
+ [Troubleshooting Systems Manager Automation](automation-troubleshooting.md)