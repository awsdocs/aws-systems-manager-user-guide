# Configuring EventBridge for Systems Manager events<a name="monitoring-systems-manager-events"></a>

You can use Amazon EventBridge to perform a target event when supported AWS Systems Manager status changes, state changes, or other conditions occur\. You can create a rule that runs whenever there is a state or status transition, or when there is a transition to one or more states that are of interest\. 

The following procedure provides general steps for creating an EventBridge rule that engages when a specified event is emitted by Systems Manager\. For a list of procedures in this user guide that address specific scenarios, see **Related content** at the end of this topic\.

**Note**  
When a service in your AWS account emits an event, it always goes to your account’s default event bus\. To write a rule that responds to events from AWS services in your account, associate it with the default event bus\. You can create a rule on a custom event bus that looks for events from AWS services, but this rule only engages when you receive such an event from another account through cross\-account event delivery\. For more information, see [Sending and receiving Amazon EventBridge events between AWS accounts](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-cross-account.html) in the *Amazon EventBridge User Guide*\.

**To configure EventBridge for Systems Manager events**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**, and then choose **Create rule**\.

   \-or\-

   If the EventBridge home page opens first, choose **Create rule**\.

1. Enter a name and description for the rule\.

   A rule can't have the same name as another rule in the same AWS Region and on the same event bus\.

1. For **Define pattern**, choose **Event pattern**\.

1. For **Event matching pattern**, choose **Pre\-defined pattern by service**\.

1. For **Service provider**, choose **`AWS`**\.

1. For **Service name**, choose **Systems Manager**\.

1. For **Event type**, do one of the following: 
   + Choose **All Events**\. 

     If you choose **All Events**, all events emitted by Systems Manager will match the rule\. Be aware that this option can result in many event target actions\.
   + Choose the type of Systems Manager event to use for this rule\. EventBridge supports events from the following AWS Systems Manager capabilities: 
     + Automation
     + Change Calendar
     + Compliance
     + Inventory
     + Maintenance Windows
     + Parameter Store
     + Run Command
     + State Manager
**Note**  
For Systems Manager actions that aren't supported by EventBridge, you can choose an AWS API call through CloudTrail to create an event rule that is based on an API call, which are recorded by CloudTrail\. For an example, see [Monitoring session activity using Amazon EventBridge \(console\) ](session-manager-auditing.md#session-manager-auditing-eventbridge-events)\. 

   \(Optional\) If you want to customize the event pattern, choose **Edit** next to **Event pattern**, make your changes, and choose **Save**\.

1. If you chose a Systems Manager capability in step 8, do one of the following:
   + For targets to be invoked when any supported event type for the selected capability occurs, choose **Any detail type**\.
   + For targets to be invoked when only certain event types for the selected capability occur, choose **Specific detail type\(s\)**, and then choose one or more types from the list\.

     For complete details about supported detail types, see [Reference: Amazon EventBridge event patterns and types for Systems Manager](reference-eventbridge-events.md)\.

1. If you chose a Systems Manager capability in step 8, choose whether to invoke targets for all or only certain detail types, statuses, or other supported options\. The available options depend on the capability you have selected\.

1. In the **Select event bus** area, choose the event bus that you want to associate with this rule\. If you want this rule to respond to matching events that come from your own AWS account, select **AWS default event bus**\. When an AWS service in your account emits an event, it always goes to your account’s default event bus\. 

1. In the **Select targets** area, choose the AWS service that is to act when an event of the selected type is detected\.

1. In the other fields in this section, enter information specific to this target type, if any is needed\. 

1. For many target types, EventBridge needs permissions to send events to the target\. In these cases, EventBridge can create the AWS Identity and Access Management \(IAM\) role needed for your rule to run: 
   + To create an IAM role automatically, choose **Create a new role for this specific resource**\.
   + To use an IAM role that you created earlier, choose **Use existing role**\.

1. \(Optional\) Choose **Add target** to add another target for this rule\.

1. \(Optional\) In the **Tags** area, enter one or more tags for the rule\. For more information, see [EventBridge tagging](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-tagging.html) in the *Amazon EventBridge User Guide*\.

1. Choose **Create**\.

**Related content**
+ [Creating an EventBridge event that uses a runbook \(console\)](automation-cwe-target.md#automation-cwe-target-console)
+ [Using input transformers with Automation](automation-transformers.md)
+ [Remediating compliance issues using EventBridge](sysman-compliance-fixing.md)
+ [Viewing inventory delete actions in EventBridge](sysman-inventory-custom.md#sysman-inventory-delete-cwe)
+ [Configuring EventBridge to automatically create OpsItems for specific events](OpsCenter-automatically-create-OpsItems-2.md)
+ [Configuring EventBridge rules for parameters and parameter policies](sysman-paramstore-cwe.md#cwe-parameter-changes)