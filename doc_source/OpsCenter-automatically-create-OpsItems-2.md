# Configuring EventBridge to automatically create OpsItems for specific events<a name="OpsCenter-automatically-create-OpsItems-2"></a>

Use the following procedure to configure **Systems Manager OpsItems** as the target of an Amazon EventBridge event\. When EventBridge receives the event, it creates a new OpsItem in OpsCenter\. This procedure describes how to update an *existing* EventBridge event rule\. For information about how to create a new event rule, see [Creating a rule for an AWS service](https://docs.aws.amazon.com/eventbridge/latest/userguide/create-eventbridge-rule.html) in the *Amazon EventBridge User Guide*\.

**To configure OpsCenter as a target of an EventBridge event**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**\.

1. On the **Rules** page, for **Event bus**, choose **default**\.

1. Choose a rule\.

1. In the **Rule details** section, verify that **Status** is set to **Enabled**\.

   To update the status, choose **Edit** in the upper\-right corner of the page, and then turn on **Enable the rule on the selected event bus**\.

1. On the **Targets** tab, choose **Edit**\.

1. For **Select a target**, choose **Systems Manager OpsItem**\.

1. For many target types, EventBridge needs permissions to send events to the target\. In these cases, EventBridge can create the AWS Identity and Access Management \(IAM\) role needed for your rule to run: 
   + To create an IAM role automatically, choose **Create a new role for this specific resource**\.
   + To use an IAM role that you created to give EventBridge permission to create OpsItems in OpsCenter, choose **Use existing role**\.

   For more information about the required role and permissions, see [Getting started with OpsCenter](OpsCenter-getting-started.md)\.

1. In the **Additional settings** section, for **Configure target input**, choose **Input Transformer**\.

   You can use the **Input transformer** option to specify a deduplication string and other important information for OpsItems such as a title and a severity\.

1. Choose **Configure input transformer**\.

1. In the **Target input transformer** section, for **Input path**, specify the values to parse from the triggering event\. For example, to parse the start time, end time, and other details from the event that triggers the rule, use the following JSON\.

   ```
   {
       "end-time": "$.detail.EndTime",
       "failure-cause": "$.detail.cause",
       "resources": "$.resources",
       "source": "$.detail.source",
       "start-time": "$.detail.StartTime"
   }
   ```

1. For **Template**, specify the information to send to the target\. For example, use the following JSON to pass information to OpsCenter\. The information is used to create an OpsItem\.

   ```
   {
       "title": "EBS snapshot copy failed",
       "description": "CloudWatch Event Rule SSMOpsItems-EBS-snapshot-copy-failed was triggered. Your EBS snapshot copy has failed. See below for more details.",
       "category": "Availability",
       "severity": "2",
       "source": "EC2",
       "resources": "<resources>",
       "operationalData": {
           "/aws/dedup": {
               "type": "SearchableString",
               "value": "{\"dedupString\":\"SSMOpsItems-EBS-snapshot-copy-failed\"}"
           },
           "/aws/automations": {
               "value": "[ { \"automationType\": \"AWS:SSM:Automation\", \"automationId\": \"AWS-CopySnapshot\" } ]"
           },
           "failure-cause": {
               "value": "<failure-cause>"
           },
           "source": {
               "value": "<source>"
           },
           "start-time": {
               "value": "<start-time>"
           },
           "end-time": {
               "value": "<end-time>"
           }
       }
   }
   ```

   For more information about these fields, see [Transforming target input](https://docs.aws.amazon.com/eventbridge/latest/userguide/transform-input.html) in the *Amazon EventBridge User Guide*\.

1. Choose **Confirm**\.

1. Choose **Next**\.

1. Choose **Next**\.

1. Choose **Update rule**\.

After an OpsItem is created from an event, you can view the event details by opening the OpsItem and scrolling down to the **Private operational data** section\.

For information about how to configure the options in an OpsItem, see [Working with OpsItems](OpsCenter-working-with-OpsItems.md)\.