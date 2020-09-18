# Reference: Amazon EventBridge event patterns and types for Systems Manager<a name="reference-eventbridge-events"></a>


|  | 
| --- |
| Amazon EventBridge is the preferred way to manage your events\. CloudWatch Events and EventBridge are the same underlying service and API, but EventBridge provides more features\. Changes you make in either CloudWatch or EventBridge will appear in each console\. For more information, see the [https://docs.aws.amazon.com/eventbridge/latest/userguide/](https://docs.aws.amazon.com/eventbridge/latest/userguide/)\. | 

Using Amazon EventBridge \(EventBridge\), you can create *rules* that match incoming *events* and route them to *targets* for processing\. 

An event indicates a change in an environment in your own applications, software\-as\-a\-service \(SaaS\) applications, or an AWS service\. After an event type that is specified in a rule is detected, EventBridge routes it to a specified target for processing\. Targets can include Amazon Elastic Compute Cloud \(Amazon EC2\) instances, AWS Lambda functions, Amazon Kinesis streams, Amazon Elastic Container Service \(Amazon ECS\) tasks, AWS Step Functions state machines, Amazon Simple Notification Service \(Amazon SNS\) topics, Amazon Simple Queue Service \(Amazon SQS\) queues, built\-in targets and many more\.

For information about creating EventBridge rules, see the following topics:
+ [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md)
+ [AWS Systems Manager Events](https://docs.aws.amazon.com/eventbridge/latest/userguide/event-types.html#ssm-event-types) in the *Amazon EventBridge User Guide*\.
+ [Getting Started with Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-getting-set-up.html) in the *Amazon EventBridge User Guide*

The remainder of this topic describes the types of Systems Manager events that you can include in your EventBridge rules\.

## Event type: Automation<a name="event-type-automation"></a>


****  

| Event type name  | Description of events you can add to a rule | 
| --- | --- | 
| EC2 Automation Execution Status\-change Notification | The overall status of an Automation workflow changes\. You can add one or more of the following status changes to an event rule:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-eventbridge-events.html) | 
| EC2 Automation Step Status\-change Notification | The status of a specific step in an Automation workflow changes\. You can add one or more of the following status changes to an event rule:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-eventbridge-events.html) | 

## Event type: Configuration Compliance<a name="event-type-configuration-compliance"></a>


****  

| Event type name | Description of events you can add to a rule | 
| --- | --- | 
| Configuration Compliance State Change | The state of a managed instance changes, for either Association compliance or patch compliance\. You can add one or more of the following state changes to an event rule:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-eventbridge-events.html) | 

## Event type: Inventory<a name="event-type-inventory"></a>


****  

| Event type name | Description of events you can add to a rule | 
| --- | --- | 
| Inventory Resource State Change | The state of a resource that is tracked by Systems Manager Inventory changes\.  | 

## Event type: State Manager<a name="event-type-state-manager"></a>


****  

| Event type name | Description of events you can add to a rule | 
| --- | --- | 
| EC2 State Manager Association State Change | The overall state of an Association changes as it's being applied\. You can add one or more of the following state changes to an event rule:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-eventbridge-events.html) | 
| EC2 State Manager Instance Association State Change | The state of a single managed instance that is targeted by an Association changes\. You can add one or more of the following state changes to an event rule:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-eventbridge-events.html) | 

## Event type: Maintenance Window<a name="event-type-maintenance-window"></a>


****  

| Event type name | Description of events you can add to a rule | 
| --- | --- | 
| Maintenance Window Status\-change Notification | The overall status of one or more maintenance windows changes\. You can add one or more of the following state changes to an event rule:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-eventbridge-events.html) | 
| Maintenance Window Target Registration Notification | The status of one or more maintenance window targets changes\. You can add one or more of the following state changes to an event rule:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-eventbridge-events.html) | 
| Maintenance Window Execution State\-change Notification | The overall status of a maintenance window changes while it is running\. You can add one or more of the following state changes to an event rule:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-eventbridge-events.html) | 
| Maintenance Window Task Execution State\-change Notification | The state of a task in a maintenance window changes while it is running\. You can add one or more of the following state changes to an event rule:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-eventbridge-events.html) | 
| Maintenance Window Task Target Invocation State\-change Notification | The state of a maintenance window task on a specific target changes\. You can add one or more of the following state changes to an event rule:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-eventbridge-events.html) | 
| Maintenance Window Task Registration Notification | The state of one or more maintenance window tasks changes\. You can add one or more of the following state changes to an event rule:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-eventbridge-events.html) | 

## Event type: Parameter Store<a name="event-type-parameter-store"></a>


****  

| Event type name | Description of events you can add to a rule | 
| --- | --- | 
| Parameter Store Change | The state of a parameter changes\. You can add one or more of the following state changes to an event rule:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-eventbridge-events.html)For more information, see [Configuring EventBridge for parameters](sysman-paramstore-cwe.md#cwe-parameter-changes)\. | 
| Parameter Store Policy Action | A condition of an advanced parameter policy change is met\. You can add one or more of the following status changes to an event rule:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-eventbridge-events.html)For more information, see [Configuring EventBridge for parameter policies](sysman-paramstore-cwe.md#cwe-parameter-policy-status)\. | 

## Event type: Run Command<a name="event-type-run-command"></a>


****  

| Event type name | Description of events you can add to a rule | 
| --- | --- | 
| EC2 Command Invocation Status\-change Notification | The status of a command sent to an individual managed instance changes\. You can add one or more of the following status changes to an event rule:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-eventbridge-events.html) | 
| EC2 Command Status\-change Notification  | The overall status of a command changes\. You can add one or more of the following status changes to an event rule:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/reference-eventbridge-events.html) | 