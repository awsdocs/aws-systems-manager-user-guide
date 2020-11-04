# Configuring EventBridge to automatically create OpsItems for specific events<a name="OpsCenter-automatically-create-OpsItems-2"></a>

Use the following procedure to configure **SSM OpsItems** as the target of an EventBridge event\. When EventBridge receives the event, it creates a new OpsItem in OpsCenter\.

**To configure OpsCenter as a target of a CloudWatch event**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**\.

1. On the **Rules** page, either choose an existing rule and then choose **Edit**, or choose **Create rule**\.

1. After verifying or entering the details of the rule, for **Select event** bus, choose **AWS default event bus**\.

1. For **Select targets**, choose **SSM OpsItem**\. 

1. Expand **Configure input** and verify that **Matched event** is selected\.

1. Choose **Create a new role for this specific resource** to create a new role with the required permissions\. Or, choose **Use existing role** and choose the IAM role you created that gives CloudWatch permission to create OpsItems in OpsCenter\. For more information about the required role and permissions, see [Getting started with OpsCenter](OpsCenter-getting-started.md)\.

1. If creating a new rule, choose **Create**\.

   \-or\-

   If editing a rule, choose **Update**\.

1. In the navigation pane, choose **Rules**\. If you created a new rule, verify that the list includes the new rule\.

After an OpsItem is created from an event, you can view the event details by opening the OpsItem and scrolling down to the **Private operational data** section\.

For information about how to configure the options in an OpsItem, see [Working with OpsItems](OpsCenter-working-with-OpsItems.md)\.