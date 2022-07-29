# Using input transformers with Automation<a name="automation-transformers"></a>

This AWS Systems Manager Automation walkthrough shows how to use the input transformer feature of Amazon EventBridge to extract the `instance-id` of an Amazon Elastic Compute Cloud \(Amazon EC2\) instance from an instance state change event\. Automation is a capability of AWS Systems Manager\. We use the input transformer to pass that data to the `AWS-CreateImage` runbook target as the `InstanceId` input parameter\. The rule is triggered when any instance changes to the `stopped` state\.

For more information about working with input transformers, see [Tutorial: Use Input Transformer to Customize What is Passed to the Event Target](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-input-transformer-tutorial.html) in the *Amazon EventBridge User Guide*\.

**Before you begin**  
Verify that you added the required permissions and trust policy for EventBridge to your Systems Manager Automation service role\. For more information, see [Overview of Managing Access Permissions to Your EventBridge Resources](https://docs.aws.amazon.com/eventbridge/latest/userguide/iam-access-control-identity-based-eventbridge.html) in the *Amazon EventBridge User Guide*\.

**To use input transformers with Automation**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**\.

1. Choose **Create rule**\.

1. Enter a name and description for the rule\.

   A rule can't have the same name as another rule in the same Region and on the same event bus\.

1. For **Event bus**, choose the event bus that you want to associate with this rule\. If you want this rule to respond to matching events that come from your own AWS account, select **default**\. When an AWS service in your account emits an event, it always goes to your account’s default event bus\.

1. For **Rule type**, choose **Rule with an event pattern**\.

1. Choose **Next**\.

1. For **Event source**, choose **AWS events or EventBridge partner events**\.

1. In the **Event pattern** section, choose **Event pattern form**\.

1. For **Event source**, choose **AWS services**\.

1. For **AWS service**, choose **EC2**\.

1. For **Event type**, choose **EC2 Instance State\-change Notification**\.

1. For **Specific state\(s\)**, choose **stopped**\.

1. Choose **Next**\.

1. For **Target types**, choose **AWS service**\.

1. For **Select a target**, choose **Systems Manager Automation**\.

1. For **Document**, choose **AWS\-CreateImage**\.

1. In the **Configure automation parameter\(s\)** section, choose **Input Transformer**\.

1. For **Input path**, enter **\{"instance":"$\.detail\.instance\-id"\}**\.

1. For **Template**, enter **\{"InstanceId":\[<instance>\]\}**\.

1. For **Execution role**, choose **Use existing role** and choose your Automation service role\.

1. Choose **Next**\.

1. \(Optional\) Enter one or more tags for the rule\. For more information, see [Tagging Your Amazon EventBridge Resources](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-tagging.html) in the *Amazon EventBridge User Guide*\.

1. Choose **Next**\.

1. Review the details of the rule and choose **Create rule**\.