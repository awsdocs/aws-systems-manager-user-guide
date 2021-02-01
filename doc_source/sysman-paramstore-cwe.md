# Setting up notifications or trigger actions based on Parameter Store events<a name="sysman-paramstore-cwe"></a>

The topics in this section explain how to use Amazon EventBridge and Amazon Simple Notification Service \(Amazon SNS\) to notify you about changes to Systems Manager parameters\. You can create an EventBridge rule to notify you when a parameter or a parameter label version is created, updated, or deleted\. You can be notified about changes or status related to parameter policies, such as when a parameter expires, is going to expire, or hasn't changed for a specified period of time\.

**Note**  
Parameter policies are available for parameters that use the advanced parameters tier\. Charges apply\. For more information, see [Assigning parameter policies](parameter-store-policies.md) and [Managing parameter tiers](parameter-store-advanced-parameters.md)\.

The topics below also explain how to trigger other actions on a target for specific parameter events\. For example, you can run an AWS Lambda function to recreate a parameter automatically when it expires or is deleted\. You can set up a notification to trigger a Lambda function when your database password is updated\. The Lambda function can force your database connections to reset or reconnect with the new password\. EventBridge also supports running Run Command commands and Automations executions, and actions in many other AWS services\. For more information, see the *[Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/)*\.

**Before You Begin**  
Create any resources you need to specify the target action for the rule you create\. For example, if the rule you create is for sending a notification, first create an Amazon SNS topic\. For more information, see [Getting Started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/GettingStarted.html) in the *Amazon Simple Notification Service Developer Guide*\.

**Topics**
+ [Configuring EventBridge for parameters](#cwe-parameter-changes)
+ [Configuring EventBridge for parameter policies](#cwe-parameter-policy-status)

## Configuring EventBridge for parameters<a name="cwe-parameter-changes"></a>

This topic explains how to create an EventBridge rule that invokes a target based on events that happen to one or more parameters in your AWS account\.

**To configure EventBridge for Systems Manager parameters**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**, and then choose **Create rule**\.

   \-or\-

   If the Amazon EventBridge home page opens first, choose **Create rule**\.

1. Enter a name and description for the rule\.

   A rule can't have the same name as another rule in the same Region and on the same event bus\.

1. For **Define pattern**, choose **Event pattern**\.

1. For **Event matching pattern**, choose **Custom pattern**\.

1. For **Event pattern**, paste the following content in the box:

   ```
   {
       "source": [
           "aws.ssm"
       ],
       "detail-type": [
           "Parameter Store Change"
       ],
       "detail": {
           "name": [
               "parameter-1-name",
               "/parameter-2-name/level-2",
               "/parameter-3-name/level-2/level-3"
           ],
           "operation": [
               "Create",
               "Update",
               "Delete",
               "LabelParameterVersion"
           ]
       }
   }
   ```

1. Modify the contents for the parameters and the operations you want to take action on\. 

   For example, the following content means an action is taken when either of the parameters named /`Oncall` and `/Project/Teamlead` are updated:

   ```
   {
       "source": [
           "aws.ssm"
       ],
       "detail-type": [
           "Parameter Store Change"
       ],
       "detail": {
           "name": [
               "/Oncall",
               "/Project/Teamlead"
           ],
           "operation": [
               "Update"
           ]
       }
   }
   ```

1. Choose **Save**\.

1. For **Select event bus**, choose the event bus that you want to associate with this rule\. If you want this rule to trigger on matching events that come from your own AWS account, select ** AWS default event bus**\. When an AWS service in your account emits an event, it always goes to your account’s default event bus\. 

1. For **Select targets**, choose a target type and a supported resource\. For example, if you choose **SNS topic**, make a selection for **Topic**\. If you choose **CodePipeline**, make a selection for **Pipeline ARN**\.

1. Expand **Configure input** and choose an option\. Then provide any other configuration details required by the target type you selected\.

1. \(Optional\) Enter one or more tags for the rule\. For more information, see [Tagging Your Amazon EventBridge Resources](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-tagging.html) in the *Amazon EventBridge User Guide*\.

1. Choose **Create**\.

## Configuring EventBridge for parameter policies<a name="cwe-parameter-policy-status"></a>

This topic explains how to create EventBridge rules that invoke targets based on events that happen to one or more parameter policies in your AWS account\. When you create an advanced parameter, you specify when a parameter expires, when to receive notification before a parameter expires, and how long to wait before notification should be sent that a parameter hasn't changed\. You set up notification for these events using the following procedure\. For more information, see [Assigning parameter policies](parameter-store-policies.md) and [Managing parameter tiers](parameter-store-advanced-parameters.md)\.

**To configure EventBridge for Systems Manager parameter policies**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**, and then choose **Create rule**\.

   \-or\-

   If the Amazon EventBridge home page opens first, choose **Create rule**\.

1. Enter a name and description for the rule\.

   A rule can't have the same name as another rule in the same Region and on the same event bus\.

1. For **Define pattern**, choose **Event pattern**\.

1. For **Event matching pattern**, choose **Custom pattern**\.

1. For **Event pattern**, paste the following content in the box:

   ```
   {
       "source": [
           "aws.ssm"
       ],
       "detail-type": [
           "Parameter Store Policy Action"
       ],
       "detail": {
           "parameter-name": [
               "parameter-1-name",
               "/parameter-2-name/level-2",
               "/parameter-3-name/level-2/level-3"
           ],
           "policy-type": [
               "Expiration",
               "ExpirationNotification",
               "NoChangeNotification"
           ]
       }
   }
   ```

1. Modify the contents for the parameters and the policy types you want to take action on\. For example, the following content means an action is taken whenever the parameter named /`OncallDuties` expires and is deleted:

   ```
   {
       "source": [
           "aws.ssm"
       ],
       "detail-type": [
           "Parameter Store Policy Action"
       ],
       "detail": {
           "parameter-name": [
               "/OncallDuties"
           ],
           "policy-type": [
               "Expiration"
           ]
       }
   }
   ```

1. Choose **Save**\.

1. For **Select event bus**, choose the event bus that you want to associate with this rule\. If you want this rule to trigger on matching events that come from your own AWS account, select ** AWS default event bus**\. When an AWS service in your account emits an event, it always goes to your account’s default event bus\. 

1. For **Select targets**, choose a target type and a supported resource\. For example, if you choose **SNS topic**, make a selection for **Topic**\. If you choose **CodePipeline**, make a selection for **Pipeline ARN**\.

1. Expand **Configure input** and choose an option\. Then provide any other configuration details required by the target type you selected\.

1. \(Optional\) Enter one or more tags for the rule\. For more information, see [Tagging Your Amazon EventBridge Resources](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-tagging.html) in the *Amazon EventBridge User Guide*\.

1. Choose **Create**\.

**Related Information**
+ \(Blog post\) [Use parameter labels for easy configuration update across environments](http://aws.amazon.com/blogs/mt/use-parameter-labels-for-easy-configuration-update-across-environments/)
+ [Tutorial: Use EventBridge to Relay Events to AWS Systems Manager Run Command](https://docs.aws.amazon.com/eventbridge/latest/userguide/ec2-run-command.html) in the *Amazon EventBridge User Guide*
+ [Tutorial: Set AWS Systems Manager Automation as an EventBridge Target](https://docs.aws.amazon.com/eventbridge/latest/userguide/ssm-automation-as-target.html) in the *Amazon EventBridge User Guide*