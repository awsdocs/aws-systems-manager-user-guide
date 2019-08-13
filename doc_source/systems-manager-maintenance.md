# AWS Systems Manager Maintenance Windows<a name="systems-manager-maintenance"></a>

AWS Systems Manager Maintenance Windows let you define a schedule for when to perform potentially disruptive actions on your instances such as patching an operating system, updating drivers, or installing software or patches\. Maintenance Windows also lets you schedule actions on numerous other AWS resource types, such as Amazon Simple Storage Service \(Amazon S3\) buckets, Amazon Simple Queue Service \(Amazon SQS\) queues, AWS Key Management Service \(AWS KMS\) keys, and many more\. For a full list of supported resource types that you can include in a maintenance window target, see [Supported Resources for AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/supported-resources.html#supported-resources-console) in the *AWS Resource Groups User Guide*\.

Each maintenance window has a schedule, a maximum duration, a set of registered targets \(the instances or other AWS resources that are acted upon\), and a set of registered tasks\. You can add tags to your maintenance windows when you create or update them\. \(Tags are keys that help identify and sort your resources within your organization\.\) You can also specify dates that a maintenance window should not run before or after, and you can specify the international time zone on which to base the maintenance window schedule\. \(For an explanation of how the various schedule\-related options for maintenance windows relate to one another, see [Reference: Maintenance Windows Scheduling and Active Period Options](reference-maintenance-windows-schedule-options.md)\.\)

Maintenance windows support running four types of tasks:
+ Systems Manager Run Command commands

  For more information about Run Command, see [AWS Systems Manager Run Command](execute-remote-commands.md)\.
+ Systems Manager Automation workflows

  For more information about Automation workflows, see [AWS Systems Manager Automation](systems-manager-automation.md)\.
+ AWS Lambda functions

  For more information about Lambda functions, see [Working with Lambda Functions](https://docs.aws.amazon.com/lambda/latest/dg/lambda-introduction-function.html) in the *AWS Lambda Developer Guide*\.
+ AWS Step Functions tasks

  For more information about Step Functions, see the *[AWS Step Functions Developer Guide](https://docs.aws.amazon.com/step-functions/latest/dg/)*\.

This means you can use maintenance windows to perform tasks like the following on your selected targets:
+ Install or update applications\.
+ Apply patches\.
+ Install or update SSM Agent\.
+ Run PowerShell commands and Linux shell scripts by using a Systems Manager Run Command task\.
+ Build Amazon Machine Images \(AMIs\), boot\-strap software, and configure instances by using a Systems Manager Automation task\.
+ Run AWS Lambda functions that trigger additional actions, such as scanning your instances for patch updates\.
+ Run AWS Step Functions state machines to perform tasks such as removing an instance from an Elastic Load Balancing environment, patching the instance, and then adding the instance back to the Elastic Load Balancing environment\.
+ Target instances that are offline by specifying an AWS resource group as the target\.

**Topics**
+ [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)
+ [Working with Maintenance Windows \(Console\)](sysman-maintenance-working.md)
+ [Systems Manager Maintenance Windows Tutorials \(AWS CLI\)](maintenance-windows-tutorials.md)
+ [Maintenance Windows Walkthroughs](maintenance-window-walkthroughs.md)