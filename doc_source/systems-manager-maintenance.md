# AWS Systems Manager Maintenance Windows<a name="systems-manager-maintenance"></a>

AWS Systems Manager Maintenance Windows let you define a schedule for when to perform potentially disruptive actions on your instances such as patching an operating system, updating drivers, or installing software or patches\. Each Maintenance Window has a schedule, a duration, a set of registered targets, and a set of registered tasks\. With Maintenance Windows, you can perform tasks like the following:
+ Install or update applications\.
+ Apply patches\.
+ Install or update the SSM Agent\.
+ Run PowerShell commands and Linux shell scripts by using a Systems Manager Run Command task\.
+ Build Amazon Machine Images \(AMIs\), boot\-strap software, and configure instances by using Systems Manager Automation\.
+ Run AWS Lambda functions that trigger additional actions, such as scanning your instances for patch updates\.
+ Run AWS Step Function state machines to perform tasks such as removing an instance from an Elastic Load Balancing environment, patching the instance, and then adding the instance back to the Elastic Load Balancing environment\.

**Topics**
+ [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)
+ [Working with Maintenance Windows](sysman-maintenance-working.md)
+ [Systems Manager Maintenance Window Walkthroughs](sysman-maintenance-walk.md)