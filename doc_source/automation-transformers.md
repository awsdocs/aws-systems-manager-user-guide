# Walkthrough: Using input transformers with Automation<a name="automation-transformers"></a>

This Systems Manager Automation walkthrough shows how to use the input transformer feature of Amazon CloudWatch Events to extract the `instance-id` of an Amazon EC2 instance from an instance state change event\. We use the input transformer to pass that data to the `AWS-CreateImage` Systems Manager Automation document target as the `InstanceId` input parameter\. The rule is triggered when any instance changes to the `stopped` state\.

For more information about working with input transformers, see [Tutorial: Use Input Transformer to Customize What is Passed to the Event Target](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CloudWatch-Events-Input-Transformer-Tutorial.html) in the *Amazon CloudWatch Events User Guide*\.

**Before You Begin**  
Verify that you added the required permissions and trust policy for CloudWatch Events to your Systems Manager Automation service role\. For more information, see [Overview of Managing Access Permissions to Your CloudWatch Events Resources](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/iam-access-control-identity-based-cwe.html)\.

**To use input transformers with Automation**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Events**, and then choose **Create rule**\.

1. For **Event source**, do the following:

   1. Choose **Event Pattern**\.

   1. For **Build event pattern to match**, choose **Events by Service**\.

   1. For **Service Name**, choose **EC2**\.

   1. For **Event Type**, choose **EC2 Instance State\-change Notification**\.

   1. Choose **Specific state\(s\)** and **stopped** from the dropdown\.

   1. Choose **Any instance**\.

1. For **Targets**, choose **Add target**, **SSM Automation**\.

1. For **Document**, choose **AWS\-CreateImage**\.

1. Choose **Configure automation parameter\(s\)**, **Input Transformer**\.

1. In the first box under **Input Transformer**, enter **\{"instance":"$\.detail\.instance\-id"\}**\.

1. In the second box, enter **\{"InstanceId":\[<instance>\]\}**\.

1. Choose **Use existing role** and choose your Automation service role\.

1. Choose **Configure details**\.

1. Enter a **Name** and **Description** for the rule, and choose **Create rule**\.