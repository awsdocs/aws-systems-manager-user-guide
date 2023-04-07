# Remediating compliance issues using EventBridge<a name="sysman-compliance-fixing"></a>

You can quickly remediate patch and association compliance issues by using Run Command, a capability of AWS Systems Manager\. You can target instance or AWS IoT Greengrass core device IDs or tags and run the `AWS-RunPatchBaseline` document or the `AWS-RefreshAssociation` document\. If refreshing the association or re\-running the patch baseline fails to resolve the compliance issue, then you need to investigate your associations, patch baselines, or instance configurations to understand why the Run Command operations didn't resolve the problem\. 

For more information about patching, see [AWS Systems Manager Patch Manager](patch-manager.md) and [About the `AWS-RunPatchBaseline` SSM document](patch-manager-aws-runpatchbaseline.md)\.

For more information about associations, see [Working with associations in Systems Manager](systems-manager-associations.md)\.

For more information about running a command, see [AWS Systems Manager Run Command](run-command.md)\.

**Specify Compliance as the target of an EventBridge event**  
You can also configure Amazon EventBridge to perform an action in response to Systems Manager Compliance events\. For example, if one or more managed nodes fail to install Critical patch updates or run an association that installs anti\-virus software, then you can configure EventBridge to run the `AWS-RunPatchBaseline` document or the `AWS-RefreshAssocation` document when the Compliance event occurs\. 

Use the following procedure to configure Compliance as the target of an EventBridge event\.

**To configure Compliance as the target of a EventBridge event \(console\)**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**\.

1. Choose **Create rule**\.

1. Enter a name and description for the rule\.

   A rule can't have the same name as another rule in the same AWS Region and on the same event bus\.

1. For **Event bus**, choose the event bus that you want to associate with this rule\. If you want this rule to respond to matching events that come from your own AWS account, select **default**\. When an AWS service in your account emits an event, it always goes to your account’s default event bus\.

1. For **Rule type**, choose **Rule with an event pattern**\.

1. Choose **Next**\.

1. For **Event source**, choose **AWS events or EventBridge partner events**\.

1. In the **Event pattern** section, choose **Event pattern form**\.

1. For **Event source**, choose **AWS services**\.

1. For **AWS service**, choose **Systems Manager**\.

1. For **Event type**, choose **Configuration Compliance**\.

1. For **Specific detail type\(s\)**, choose **Configuration Compliance State Change**\.

1. Choose **Next**\.

1. For **Target types**, choose **AWS service**\.

1. For **Select a target**, choose **Systems Manager Run Command**\.

1. In the **Document** list, choose a Systems Manager document \(SSM document\) to run when your target is invoked\. For example, choose `AWS-RunPatchBaseline` for a non\-compliant patch event, or choose `AWS-RefreshAssociation` for a non\-compliant association event\.

1. Specify information for the remaining fields and parameters\.
**Note**  
Required fields and parameters have an asterisk \(\*\) next to the name\. To create a target, you must specify a value for each required parameter or field\. If you don't, the system creates the rule, but the rule won't be run\.

1. Choose **Next**\.

1. \(Optional\) Enter one or more tags for the rule\. For more information, see [Tagging Your Amazon EventBridge Resources](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-tagging.html) in the *Amazon EventBridge User Guide*\.

1. Choose **Next**\.

1. Review the details of the rule and choose **Create rule**\.