# Configuring EventBridge for Systems Manager events<a name="monitoring-systems-manager-events"></a>

You can use Amazon EventBridge to perform a target event when supported AWS Systems Manager status changes, state changes, or other conditions occur\. You can create a rule that runs whenever there is a state or status transition, or when there is a transition to one or more states that are of interest\. 

The following procedure provides general steps for creating an EventBridge rule that engages when a specified event is emitted by Systems Manager\. For a list of procedures in this user guide that address specific scenarios, see **Related content** at the end of this topic\.

**Note**  
When an AWS service in your account emits an event, it always goes to your account’s default event bus\. To write a rule that responds to events from AWS services in your account, you must associate it with the default event bus\. You can create a rule on a custom event bus that looks for events from AWS services, but this rule will engage only when you receive such an event from another account via cross\-account event delivery\. For more information, see [Sending and Receiving Events Between AWS Accounts](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-cross-account-event-delivery.html) in the *Amazon EventBridge User Guide*\.

**To configure EventBridge for Systems Manager events**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**, and then choose **Create rule**\.

   \-or\-

   If the Amazon EventBridge home page opens first, choose **Create rule**\.

1. Enter a name and description for the rule\.

   A rule can't have the same name as another rule in the same AWS Region and on the same event bus\.

1. For **Define pattern**, choose **Event pattern**\.

1. Choose **Pre\-defined pattern by service**\.

1. For **Service provider**, choose **AWS**\.

1. For **Service name**, choose **EC2 Simple Systems Manager \(SSM\)**\.

1. For **Event type**, do one of the following: 
   + Choose **All Events**\. 

     If you choose **All Events**, all events emitted by Systems Manager will match the rule\. Be aware that this option can result in a very large number of event target actions\.
   + Choose the type of Systems Manager event to use for this rule\. EventBridge currently supports events from the following Systems Manager capabilities: 
     + Configuration Compliance
     + Inventory
     + Maintenance Windows
     + Parameter Store
     + Run Command
     + State Manager
**Note**  
For Systems Manager actions that aren't supported by EventBridge, you can choose AWS API Call via CloudTrail to create an event rule that is based on an API call, which are recorded by AWS CloudTrail\. For an example, see [ Monitoring session activity using Amazon EventBridge \(console\) ](session-manager-logging-auditing.md#session-manager-logging-auditing-eventbridge-events)\. 

   \(Optional\) If you want to customize the event pattern, choose **Edit** next to **Event pattern**, make your changes, and choose **Save**\.

1. If you chose a Systems Manager capability in step 8, do one of the following:
   + For targets to be invoked when any supported event type for the selected capability occurs, choose **Any detail type**\.
   + For targets to be invoked when only certain event types for the selected capability occur, choose **Specific detail type\(s\)**, and then choose one or more types from the list\.

     For complete details about supported detail types, see [Reference: Amazon EventBridge event patterns and types for Systems Manager](reference-eventbridge-events.md) and [AWS Systems Manager Events](https://docs.aws.amazon.com/eventbridge/latest/userguide/event-types.html#ssm-event-types) in the *Amazon EventBridge User Guide*\.

1. If you chose a Systems Manager capability in step 8, choose whether to invoke targets for all or only certain detail types, statuses, or other supported options\. The available options depend on the capability you have selected\.

1. For **Select event bus**, choose the event bus that you want to associate with this rule\. If you want this rule to respond to matching events that come from your own AWS account, select ** AWS default event bus**\. When an AWS service in your account emits an event, it always goes to your account’s default event bus\. 

1. For **Select targets**, choose the AWS service that is to act when an event of the selected type is detected\.

1. In the other fields in this section, enter information specific to this target type, if any is needed\. 

1. For many target types, EventBridge needs permissions to send events to the target\. In these cases, EventBridge can create the AWS Identity and Access Management \(IAM\) role needed for your rule to run: 
   + To create an IAM role automatically, choose **Create a new role for this specific resource**\.
   + To use an IAM role that you created earlier, choose **Use existing role**\.

1. Optionally, choose **Add target** to add another target for this rule\.

1. \(Optional\) Enter one or more tags for the rule\. For more information, see [Tagging Your Amazon EventBridge Resources](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-tagging.html) in the *Amazon EventBridge User Guide*\.

1. Choose **Create**\.

**Related content**
+ [Creating an EventBridge event that runs an automation \(console\)](automation-cwe-target.md#automation-cwe-target-console)
+ [Walkthrough: Using input transformers with Automation](automation-transformers.md)
+ [Remediating compliance issues using EventBridge](sysman-compliance-fixing.md)
+ [Viewing inventory delete actions in EventBridge](sysman-inventory-custom.md#sysman-inventory-delete-cwe)
+ [Configuring EventBridge to automatically create OpsItems for specific events](OpsCenter-automatically-create-OpsItems-2.md)
+ [Configuring EventBridge for parameters](sysman-paramstore-cwe.md#cwe-parameter-changes)
+ [Configuring EventBridge for parameter policies](sysman-paramstore-cwe.md#cwe-parameter-policy-status)