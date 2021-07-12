# Configuring EventBridge to automatically create OpsItems for specific events<a name="OpsCenter-automatically-create-OpsItems-2"></a>

Use the following procedure to configure **SSM OpsItems** as the target of an Amazon EventBridge event\. When EventBridge receives the event, it creates a new OpsItem in OpsCenter\. This procedure describes how to update an *existing* EventBridge event rule\. For information about how to create a new event rule, see [Creating a rule for an AWS service](https://docs.aws.amazon.com/eventbridge/latest/userguide/create-eventbridge-rule.html) in the *Amazon EventBridge User Guide*\.

**To configure OpsCenter as a target of an EventBridge event**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**\.

1. On the **Rules** page, either choose an existing rule and then choose **Edit**, or choose **Create rule**\.

1. On the **Rules** page, choose an existing rule and then choose **Edit**\. 

1. In the **Select event bus** section, verify that **AWS default event bus** is selected and **Enable the rule on the selected event bus** option is toggled on\.

1. In the **Select targets** section, use the **Target** list to choose **SSM OpsItem**\. 

1. Expand **Configure input** and choose either **Matched events** or **Input transformer**\.
**Note**  
We recommend that you choose **Input transformer**\. This option allows you to specify a deduplication string and other important information for OpsItems such as a title and a severity\.

   If you choose **Input transformer**, enter information in the **Input Path** and **Input Template** boxes\. Here's an example of how to enter the input path\.

   ```
   {
           "end-time": "$.detail.EndTime",
           "failure-cause": "$.detail.cause",
           "resources": "$.resources",
           "source": "$.detail.source",
           "start-time": "$.detail.StartTime"
       }
   ```

   Here's an example of how to enter the input template\.

   ```
   {
       "title": "EBS snapshot copy failed",
       "description": "CloudWatch Event Rule SSMOpsItems-EBS-snapshot-copy-failed was triggered. Your EBS snapshot copy has failed. See below for more details.",
       "category": "Availability",
       "severity": "2",
       "source": "EC2",
       "resources": "resources",
       "operationalData": {
           "/aws/dedup": {
               "type": "SearchableString",
               "value": "{\"dedupString\":\"SSMOpsItems-EBS-snapshot-copy-failed\"}"
           },
           "/aws/automations": {
               "value": "[ { \"automationType\": \"AWS:SSM:Automation\", \"automationId\": \"AWS-CopySnapshot\" } ]"
           },
           "failure-cause": {
               "value": "failure-cause"
           },
           "source": {
               "value": "source"
           },
           "start-time": {
               "value": "start-time"
           },
           "end-time": {
               "value": "end-time"
           }
       }
   }
   ```

   For more information about these fields, see [Transforming target input](https://docs.aws.amazon.com/eventbridge/latest/userguide/transform-input.html) in the *Amazon EventBridge User Guide*\.

1. Choose **Create a new role for this specific resource** to create a new role with the required permissions\. Or, choose **Use existing role** and choose the IAM role you created that gives EventBridge permission to create OpsItems in OpsCenter\. For more information about the required role and permissions, see [Getting started with OpsCenter](OpsCenter-getting-started.md)\.

1. Choose **Update**\.

1. In the navigation pane, choose **Rules**\. If you created a new rule, verify that the list includes the new rule\.

After an OpsItem is created from an event, you can view the event details by opening the OpsItem and scrolling down to the **Private operational data** section\.

For information about how to configure the options in an OpsItem, see [Working with OpsItems](OpsCenter-working-with-OpsItems.md)\.