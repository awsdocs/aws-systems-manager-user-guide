# Walkthrough: Using input transformers with Automation<a name="automation-transformers"></a>

This Systems Manager Automation walkthrough shows how to use the input transformer feature of Amazon EventBridge to extract the `instance-id` of an EC2 instance from an instance state change event\. We use the input transformer to pass that data to the `AWS-CreateImage` Systems Manager Automation document target as the `InstanceId` input parameter\. The rule is triggered when any instance changes to the `stopped` state\.

For more information about working with input transformers, see [Tutorial: Use Input Transformer to Customize What is Passed to the Event Target](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-input-transformer-tutorial.html) in the *Amazon EventBridge User Guide*\.

**Before You Begin**  
Verify that you added the required permissions and trust policy for EventBridge to your Systems Manager Automation service role\. For more information, see [Overview of Managing Access Permissions to Your EventBridge Resources](https://docs.aws.amazon.com/eventbridge/latest/userguide/iam-access-control-identity-based-eventbridge.html) in the *Amazon EventBridge User Guide*\.

**To use input transformers with Automation**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**, and then choose **Create rule**\.

   \-or\-

   If the Amazon EventBridge home page opens first, choose **Create rule**\.

1. Enter a name and description for the rule\.

   A rule can't have the same name as another rule in the same Region and on the same event bus\.

1. For **Define pattern**, do the following:

   1. Choose **Event pattern**\.

   1. For **Event matching pattern**, choose **Pre\-defined pattern by service**\.

   1. For **Event matching pattern**, choose **Pre\-defined pattern by service**\.

   1. For **Service provider**, choose **AWS**\.

   1. For **Service name**, choose **EC2**\.

   1. For **Event type**, choose **EC2 Instance State\-change Notification**\.

   1. Choose **Specific state\(s\)** and **stopped** from the dropdown\.

   1. Choose **Any instance**\.

1. For **Select event bus**, choose the event bus that you want to associate with this rule\. If you want this rule to trigger on matching events that come from your own AWS account, select ** AWS default event bus**\. When an AWS service in your account emits an event, it always goes to your account’s default event bus\. 

1. For **Target**, choose **SSM Automation**\.

1. For **Document**, choose **AWS\-CreateImage**\.

1. Expand **Configure automation parameter\(s\)** and choose **Input Transformer**\.

1. In the first box under **Input Transformer**, enter **\{"instance":"$\.detail\.instance\-id"\}**\.

1. In the second box, enter **\{"InstanceId":\[<instance>\]\}**\.

1. Choose **Use existing role** and choose your Automation service role\.

1. \(Optional\) Enter one or more tags for the rule\. For more information, see [Tagging Your Amazon EventBridge Resources](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-tagging.html) in the *Amazon EventBridge User Guide*\.

1. Choose **Create**\.