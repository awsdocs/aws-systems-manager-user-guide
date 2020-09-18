# Configuring EventBridge to automatically create OpsItems for specific events<a name="OpsCenter-automatically-create-OpsItems-2"></a>

Use the following procedure to configure **SSM OpsItems** as the target of an EventBridge event\. When EventBridge receives the event, it creates a new OpsItem in OpsCenter\.

**To configure OpsCenter as a target of a CloudWatch event**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rule**, and then either choose to create a new rule or edit an existing rule\.

1. After verifying the details of the rule, for **Select event** bus, choose **AWS default event bus**\.

1. For **Target** list, choose **SSM OpsItem**\. 

1. Expand **Configure input** and verify that **Matched event** is selected\.

1. Choose **Create a new role for this specific resource** to create a new role with the required permissions\. Or, choose **Use existing role** and choose the IAM role you created that gives CloudWatch permission to create OpsItems in OpsCenter\. For more information about the required role and permissions, see [Getting started with OpsCenter](OpsCenter-getting-started.md)\.

1. If creating a new rule, choose **Create**\.

   \-or\-

   If editing a rule, choose **Update**\.

1. In the navigation pane, choose **Rules**\. If you created a new rule, verify that the list includes a new rule that begins with `AwsSSMOpsCenter-*`, such as `AwsSSMOpsCenter-EC2` or `AwsSSMOpsCenter-DynamoDB`\.

After an OpsItem is created from an event, you can view the event details by opening the OpsItem and scrolling down to the **Private operational data** section\.

![\[Viewing operational data to view event details.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_2.png)

For information about how to configure the options in an OpsItem, see [Working with OpsItems](OpsCenter-working-with-OpsItems.md)\.