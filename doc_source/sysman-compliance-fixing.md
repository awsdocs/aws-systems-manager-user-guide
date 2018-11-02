# Remediating Compliance Issues<a name="sysman-compliance-fixing"></a>

You can quickly remediate patch and association compliance issues by using Systems Manager Run Command\. You can target either instance IDs or Amazon EC2 tags and run the AWS\-RunPatchBaseline document or the AWS\-RefreshAssociation document\. If refreshing the association or re\-running the patch baseline fails to resolve the compliance issue, then you need to investigate your associations, patch baselines, or instance configurations to understand why the Run Command executions did not resolve the problem\. 

For more information about patching, see [AWS Systems Manager Patch Manager](systems-manager-patch.md) and [About the SSM Document AWS\-RunPatchBaseline](patch-manager-about-aws-runpatchbaseline.md)\.

For more information about associations, see [Working with Associations in Systems Manager](systems-manager-associations.md)\.

For more information about running a command, see [Running Commands Using Systems Manager Run Command](run-command.md)\.

**Specify Configuration Compliance as the target of a CloudWatch event**  
You can also configure CloudWatch Events to perform an action in response to Configuration Compliance events\. For example, if one or more instances fail to install Critical patch updates or run an association that installs anti\-virus software, then you can configure CloudWatch to run the AWS\-RunPatchBaseline document or the AWS\-RefreshAssocation document when the Configuration Compliance event occurs\. 

Use the following procedure to configure Configuration Compliance as the target of a CloudWatch event\.

**To configure Configuration Compliance as the target of a CloudWatch event**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Events**, and then choose **Create rule**\.

1. Choose **Event Pattern**\. **Event Pattern** lets you build a rule that generates events for specific actions in AWS services\. 

1. In the **Service Name** field, choose **EC2 Simple Systems Manager \(SSM\)**

1. In the **Event Type** field, choose **Configuration Compliance**\.

1. Choose **Add target**\.

1. In the **Select target type** list, choose **SSM Run Command**\. 

1. In the **Document** list, choose an SSM document to run when your target is invoked\. For example, choose choose **AWS\-RunPatchBaseline** for a non\-compliant patch event, or choose **AWS\-RefreshAssociation** for a non\-compliant association event\.

1. Specify information for the remaining fields and parameters\.
**Note**  
Required fields and parameters have an asterisk \(\*\) next to the name\. To create a target, you must specify a value for each required parameter or field\. If you don't, the system creates the rule, but the rule won't be run\.

1. Choose **Configure details** and complete the wizard\.