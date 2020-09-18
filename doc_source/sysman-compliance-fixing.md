# Remediating compliance issues using EventBridge<a name="sysman-compliance-fixing"></a>

You can quickly remediate patch and association compliance issues by using Systems Manager Run Command\. You can target either instance IDs or Amazon EC2 tags and run the `AWS-RunPatchBaseline` document or the `AWS-RefreshAssociation` document\. If refreshing the association or re\-running the patch baseline fails to resolve the compliance issue, then you need to investigate your associations, patch baselines, or instance configurations to understand why the Run Command executions did not resolve the problem\. 

For more information about patching, see [AWS Systems Manager Patch Manager](systems-manager-patch.md) and [About the SSM document AWS\-RunPatchBaseline](patch-manager-about-aws-runpatchbaseline.md)\.

For more information about associations, see [Working with associations in Systems Manager](systems-manager-associations.md)\.

For more information about running a command, see [Running commands using Systems Manager Run Command](run-command.md)\.

**Specify Configuration Compliance as the target of an EventBridge event**  
You can also configure EventBridge to perform an action in response to Configuration Compliance events\. For example, if one or more instances fail to install Critical patch updates or run an association that installs anti\-virus software, then you can configure EventBridge to run the `AWS-RunPatchBaseline` document or the `AWS-RefreshAssocation` document when the Configuration Compliance event occurs\. 

Use the following procedure to configure Configuration Compliance as the target of an EventBridge event\.

**To configure Configuration Compliance as the target of a EventBridge event \(console\)**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**, and then choose **Create rule**\.

   \-or\-

   If the Amazon EventBridge home page opens first, choose **Create rule**\.

1. Enter a name and description for the rule\.

   A rule can't have the same name as another rule in the same Region and on the same event bus\.

1. For **Define pattern**, choose **Event pattern**\. **Event pattern** lets you build a rule that generates events for specific actions in AWS services\.

1. Choose **Pre\-defined pattern by service**\.

1. For **Service provider**, choose **AWS**\.

1. For **Service Name**, choose **EC2 Simple Systems Manager \(SSM\)**

1. For **Event type**, choose **Configuration Compliance**\.

1. For **Select event bus**, choose the event bus that you want to associate with this rule\. If you want this rule to trigger on matching events that come from your own AWS account, select ** AWS default event bus**\. When an AWS service in your account emits an event, it always goes to your account’s default event bus\. 

1. For **Target**, choose **SSM Run Command**\. 

1. In the **Document** list, choose an SSM document to run when your target is invoked\. For example, choose `AWS-RunPatchBaseline` for a non\-compliant patch event, or choose `AWS-RefreshAssociation` for a non\-compliant association event\.

1. Specify information for the remaining fields and parameters\.
**Note**  
Required fields and parameters have an asterisk \(\*\) next to the name\. To create a target, you must specify a value for each required parameter or field\. If you don't, the system creates the rule, but the rule won't be run\.

1. \(Optional\) Enter one or more tags for the rule\. For more information, see [Tagging Your Amazon EventBridge Resources](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-tagging.html) in the *Amazon EventBridge User Guide*\.

1. Choose **Create** and complete the wizard\.