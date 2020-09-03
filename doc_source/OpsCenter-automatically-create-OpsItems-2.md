# Configuring CloudWatch Events to automatically create OpsItems for specific events<a name="OpsCenter-automatically-create-OpsItems-2"></a>

Use the following procedure to configure **SSM OpsItems** as the target of a CloudWatch event\. When CloudWatch receives the event, it creates a new OpsItem\.

**To configure OpsCenter as a target of a CloudWatch event**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Events**, and then either choose to create a new rule or edit an existing rule\.

1. After verifying the details of the rule, choose **Add target**\.

1. In the **Select target type** list, choose **SSM OpsItem**\. 

1. For **Configure input**, verify that **Matched event** is selected\.

1. In the permissions section, choose **Create a new role for this specific resource** to create a new role with the required permissions\. Or, choose **Use existing role** and choose the IAM role you created that gives CloudWatch permission to create OpsItems in OpsCenter\. For more information about the required role and permissions, see [Getting started with OpsCenter](OpsCenter-getting-started.md)\.

1. Choose **Configure details**\. CloudWatch opens the **Step 2: Configure rule details** page\.

1. For **Name**, type a descriptive name that identifies the purpose of this rule\.

1. For **Description**, type information about this rule\.

1. Verify that the **Enabled** option is selected, and then choose **Create rule**\.

1. In the navigation pane, choose **Rules**\. Verify that the list includes a new rule that begins with AwsSSMOpsCenter\-\*, such as AwsSSMOpsCenter\-EC2 or AwsSSMOpsCenter\-DynamoDB\.

After an OpsItem is created from an event, you can view the event details by opening the OpsItem and scrolling down to the **Private operational data** section\.

![\[Viewing operational data to view event details.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_2.png)

For information about how to configure the options in an OpsItem, see [Working with OpsItems](OpsCenter-working-with-OpsItems.md)\.