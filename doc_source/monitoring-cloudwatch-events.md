# Monitoring Systems Manager events with Amazon CloudWatch Events<a name="monitoring-cloudwatch-events"></a>

You can configure rules in Amazon CloudWatch Events to alert you to changes in Systems Manager resources, and to direct CloudWatch Events to take actions based on the content of those events\. CloudWatch Events provides support for a number of events that are emitted by various Systems Manager capabilities\.

**Note**  
For Systems Manager actions that aren't supported by CloudWatch Events, you can create an event rule that is based on an API call, which are recorded by AWS CloudTrail\. For an example, see [Monitoring session activity using Amazon CloudWatch Events \(console\)](session-manager-logging-auditing.md#session-manager-logging-auditing-cloudwatch-events)\. 

For more information about how to get started with CloudWatch Events and set up rules, see [Getting Started with CloudWatch Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CWE_GettingStarted.html) in the *Amazon CloudWatch Events User Guide*\.

Following are lists of the Systems Manager event types with built\-in monitoring support in CloudWatch Events\.

**Run Command**  
Supported events include the following:  
+ Status change for a command \(applies to one or more instances\)\.
+ Status change for a command invocation \(applies to one instance only\)\. 
For more information, see [Configuring CloudWatch Events for Run Command](rc-cwe.md)\.

**Automation**  
Supported events include the following:  
+ Status change for an automation execution\.
+ Status change for a single step in an automation execution\.
For more information, see [Configuring CloudWatch Events for Systems Manager automation](automation-cwe.md)\.

**State Manager**  
Supported events include the following:  
+ State change for an association
+ State change for an instance association\.

**Configuration Compliance**  
Supported events include the following:  
+ State change for association compliance\.
+ State change for instance patch compliance\.
For more information, see [Remediating compliance issues](sysman-compliance-fixing.md)\.

**Maintenance Window**  
Supported events include the following:  
+ State change for a maintenance window \(enabled or disabled\)
+ Change in a maintenance window target registration\.
+ Change in a maintenance window task registration\.
+ State change for a maintenance window execution\.
+ State change for a maintenance window task execution\.
+ State change for a maintenance window task target invocation\.

**Parameter Store**  
Supported events include the following:  
+ A parameter is created, updated, or deleted, or a label is attached or moved from one version to another \(detail\-type: `"Parameter Store Change"`\)\.
+ A parameter has expired or been deleted, its expiration date is approaching, or its value hasn't been changed for a specified period of time \(detail\-type: `"Parameter Store Policy Action"`\)\.
For more information, see [Set up notifications or trigger actions based on Parameter Store events](sysman-paramstore-cwe.md)\.

**Inventory**  
Supported events include the following:  
+ Deletion of custom inventory item on an instance\. 
+ Availability of a delete action summary\.
+ A disabled custom inventory type is detected\.
For more information, see [Viewing inventory delete actions in CloudWatch Events](sysman-inventory-custom.md#sysman-inventory-delete-cwe)\.

For more information about the Systems Manager event types that are supported by CloudWatch Events, see the following topics in the *Amazon CloudWatch Events User Guide*:
+ [AWS Systems Manager Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html#ssm_event_types)
+ [AWS Systems Manager Configuration Compliance Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html#SSM-Configuration-Compliance-event-types)
+ [AWS Systems Manager Maintenance Windows Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html#EC2_maintenance_windows_event_types)
+ [AWS Systems Manager Parameter Store Events](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html#SSM-Parameter-Store-event-types)