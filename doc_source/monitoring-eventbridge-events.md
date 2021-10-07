# Monitoring Systems Manager events with Amazon EventBridge<a name="monitoring-eventbridge-events"></a>

Amazon EventBridge is a serverless event bus service that allows you to connect your applications with data from a variety of sources\. EventBridge delivers a stream of real\-time data from your own applications, software\-as\-a\-service \(SaaS\) applications, and AWS services and routes that data to targets such as AWS Lambda\. You can set up routing rules to determine where to send your data to build application architectures that react in real time to all of your data sources\. EventBridge allows you to build event driven architectures, which are loosely coupled and distributed\.

EventBridge was formerly called Amazon CloudWatch Events\. EventBridge includes new features that allow you to receive events from SaaS partners and your own applications\. Existing CloudWatch Events users can access their existing default bus, rules, and events in the new EventBridge console and in the CloudWatch Events console\. EventBridge uses the same CloudWatch Events API, so all of your existing CloudWatch Events API usage remains the same\. 

EventBridge can add events from dozens of AWS services to your rules, and targets from over 20 AWS services\.

EventBridge provides support for both AWS Systems Manager events and Systems Manager targets\. 

**Supported Systems Manager event types**  
Among the many types of Systems Manager events that EventBridge can detect are: 
+ A maintenance window being turned off\.
+ An Automation workflow completing successfully\. Automation is a capability of AWS Systems Manager\.
+ A managed instance being out of patch compliance\.
+ A parameter value being updated\.

EventBridge supports events from the following AWS Systems Manager capabilities:
+ Automation
+ Change Calendar \(Events are emitted on a best effort basis\.\)
+ Compliance
+ Inventory \(Events are emitted on a best effort basis\.\)
+ Maintenance Windows \(Events are emitted on a best effort basis\.\)
+ Parameter Store
+ Run Command \(Events are emitted on a best effort basis\.\)
+ State Manager \(Events are emitted on a best effort basis\.\)

For complete details about supported Systems Manager event types, see [Reference: Amazon EventBridge event patterns and types for Systems Manager](reference-eventbridge-events.md) and [Amazon EventBridge event examples for Systems Manager](monitoring-systems-manager-event-examples.md)\.

**Supported Systems Manager target types**  
EventBridge supports the following three Systems Manager capabilities as targets of an event rule:
+ Running an Automation workflow
+ Running a Run Command Command document \(Events are emitted on a best effort basis\.\)
+ Creating an OpsCenter OpsItem

For suggested ways you might use these targets, see [Amazon EventBridge target examples for Systems Manager](monitoring-systems-manager-targets.md)\.

For more information about how to get started with EventBridge and set up rules, see [Getting started with Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-get-started.html) in the *Amazon EventBridge User Guide*\. For complete information about working with EventBridge, see the [https://docs.aws.amazon.com/eventbridge/latest/userguide/](https://docs.aws.amazon.com/eventbridge/latest/userguide/)\.

**Topics**
+ [Configuring EventBridge for Systems Manager events](monitoring-systems-manager-events.md)
+ [Amazon EventBridge event examples for Systems Manager](monitoring-systems-manager-event-examples.md)
+ [Amazon EventBridge target examples for Systems Manager](monitoring-systems-manager-targets.md)