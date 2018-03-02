# AWS Systems Manager Maintenance Windows<a name="systems-manager-maintenance"></a>

AWS Systems Manager Maintenance Windows let you define a schedule for when to perform potentially disruptive actions on your instances such as patching an operating system \(OS\), updating drivers, or installing software\. Each Maintenance Window has a schedule, a duration, a set of registered targets, and a set of registered tasks\. With Maintenance Windows, you can perform tasks like the following:

+ Installing applications, updating patches, installing or updating SSM Agent, or executing PowerShell commands and Linux shell scripts by using a Systems Manager Run Command task\.

+ Building Amazon Machine Images \(AMIs\), boot\-strapping software, and configuring instances by using Systems Manager Automation\.

+ Executing AWS Lambda functions that trigger additional actions such as scanning your instances for patch updates\.

+ Running AWS Step Function state machines to perform tasks such as removing an instance from an Elastic Load Balancing environment, patching the instance, and then adding the instance back to the Elastic Load Balancing environment\.


+ [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)
+ [Working with Maintenance Windows](sysman-maintenance-working.md)
+ [Systems Manager Maintenance Window Walkthroughs](sysman-maintenance-walk.md)