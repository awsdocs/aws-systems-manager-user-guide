# Setting up notifications or trigger actions based on Parameter Store events<a name="sysman-paramstore-cwe"></a>

The topics in this section explain how to use Amazon CloudWatch Events and Amazon Simple Notification Service \(Amazon SNS\) to notify you about changes to Systems Manager parameters\. You can create a CloudWatch rule to notify you when a parameter or a parameter label version is created, updated, or deleted\. You can be notified about changes or status related to parameter policies, such as when a parameter expires, is going to expire, or hasn't changed for a specified period of time\.

**Note**  
Parameter policies are available for parameters that use the advanced parameters tier\. Charges apply\. For more information, see [Assigning parameter policies](parameter-store-policies.md) and [Managing parameter tiers](parameter-store-advanced-parameters.md)\.

The topics below also explain how to trigger other actions on a target for specific parameter events\. For example, you can run an AWS Lambda function to recreate a parameter automatically when it expires or is deleted\. You can set up a notification to trigger a Lambda function when your database password is updated\. The Lambda function can force your database connections to reset or reconnect with the new password\. CloudWatch Events also supports running Run Command commands and Automations executions, and actions in many other AWS services\. For more information, see the *[Amazon CloudWatch Events User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/)*\.

**Before You Begin**  
Create any resources you need to specify the target action for the rule you create\. For example, if the rule you create is for sending a notification, first create an Amazon SNS topic\. For more information, see [Getting Started with Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/GettingStarted.html) in the *Amazon Simple Notification Service Developer Guide*\.

**Topics**
+ [Configuring CloudWatch Events for parameters](#cwe-parameter-changes)
+ [Configuring CloudWatch Events for parameter policies](#cwe-parameter-policy-status)

## Configuring CloudWatch Events for parameters<a name="cwe-parameter-changes"></a>

This topic explains how to create a CloudWatch Events rule that invokes a target based on events that happen to one or more parameters in your AWS account\.

**To configure CloudWatch Events for Systems Manager parameters**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Events**, and then choose **Create rule**\.

1. Under **Event Source**, verify that **Event Pattern** is selected\.

1. Above the **Event Pattern Preview** field, choose **Edit**\.
**Note**  
You are modifying sample code we provide instead of using the event pattern builder fields\.

1. Replace the content in the edit box with the following:

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

1. For **Targets**, choose **Add targets**\.

1. In the **Targets** list, choose a target type\. For example, choose **Lambda function** or **SNS topic**\. 

1. Expand **Configure input** and choose an option\. Then provide any other configuration details required by the target type you selected\.

1. Scroll to the bottom of the page, if necessary, and then choose **Configure details**\.

1. Provide a name and \(optional\) description for the CloudWatch Events rule\. Leave the **Enabled** box selected to make the rule active immediately\.

1. Choose **Create rule**\.

## Configuring CloudWatch Events for parameter policies<a name="cwe-parameter-policy-status"></a>

This topic explains how to create CloudWatch Events rules that invoke targets based on events that happen to one or more parameter policies in your AWS account\. When you create an advanced parameter, you specify when a parameter expires, when to receive notification before a parameter expires, and how long to wait before notification should be sent that a parameter hasn't changed\. You set up notification for these events using the following procedure\. For more information, see [Assigning parameter policies](parameter-store-policies.md) and [Managing parameter tiers](parameter-store-advanced-parameters.md)\.

**To configure CloudWatch Events for Systems Manager parameter policies**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Events**, and then choose **Create rule**\.

1. Under **Event Source**, verify that **Event Pattern** is selected\.

1. Above the **Event Pattern Preview** field, choose **Edit**\.
**Note**  
You are modifying sample code we provide instead of using the event pattern builder fields\.

1. Replace the content in the edit box with the following:

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

1. For **Targets**, choose **Add targets**\.

1. In the **Targets** list, choose a target type\. For example, choose **Lambda function** or **SNS topic**\. 

1. Expand **Configure input** and choose an option\. Then provide any other configuration details required by the target type you selected\.

1. Scroll to the bottom of the page, if necessary, and then choose **Configure details**\.

1. Provide a name and \(optional\) description for the CloudWatch Events rule\. Leave the **Enabled** box selected to make the rule active immediately\.

1. Choose **Create rule**\.

**Related Information**
+ \(Blog post\) [Use parameter labels for easy configuration update across environments](http://aws.amazon.com/blogs/mt/use-parameter-labels-for-easy-configuration-update-across-environments/)
+ [Tutorial: Use CloudWatch Events to Relay Events to AWS Systems Manager Run Command](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EC2_Run_Command.html) in the *Amazon CloudWatch Events User Guide*
+ [Tutorial: Set AWS Systems Manager Automation as a CloudWatch Events Target](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/SSM_Automation_as_Target.html) in the *Amazon CloudWatch Events User Guide*