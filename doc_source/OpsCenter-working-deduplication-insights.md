# Working with operational insights<a name="OpsCenter-working-deduplication-insights"></a>

OpsCenter, a capability of AWS Systems Manager, automatically analyzes OpsItems in your account and generates two types of *insights* in the **Operational insights** section\. The **Duplicate OpsItems** field displays a count of insights when eight or more OpsItems have the same title for the same resource\. The **Sources generating most OpsItems** field displays a count of insights when more than 50 OpsItems have the same title\. If you choose an insight, OpsCenter displays information about the affected OpsItems and resources\. The following screenshot shows an example with the details of a duplicate OpsItem insight\. 

![\[The detailed view of an OpsCenter insight with information about duplicate OpsItems.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsCenter-insights-detailed.png)

**Note**  
Operational insights are disabled by default\. You must enable this feature, as described in this topic\.

## Viewing insights and resolving duplicate OpsItems<a name="OpsCenter-working-deduplication-insights-viewing"></a>

This section describes how to enable and view operational insights\. This section also describes how to resolve duplicate OpsItems and reduce the number of OpsItems created by a source\. Before you begin, be aware of the following important details\.
+ If you enable operational insights, OpsCenter creates OpsItems of type `insight`\. OpsCenter limits the number of insights the system creates, but your AWS account is charged for insight OpsItems created by the system\. For more information, see [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\.
+ If you enable operational insights, Systems Manager creates an AWS Identity and Access Management \(IAM\) service\-linked role called `AWSServiceRoleForAmazonSSM_OpsInsights`\. A service\-linked role is a unique type of IAM role that is linked directly to Systems Manager\. Service\-linked roles are predefined and include all the permissions the service requires to call other AWS services on your behalf\. For more information about the `AWSServiceRoleForAmazonSSM_OpsInsights` service\-linked role, [Using roles to create operational insight OpsItems in Systems Manager OpsCenter: `AWSServiceRoleForAmazonSSM_OpsInsights`](using-service-linked-roles-service-action-4.md)\.
+ OpsCenter periodically refreshes insights using a batch process\. This means the list of insights displayed in OpsCenter can be out of sync\.
+ OpsCenter has a limit of 25 insights\. The system stops creating new insights when it reaches this limit\. 
+ To resolve insights, you must first resolve all OpsItems associated with an insight\. You can use the `AWS-BulkResolveOpsItemsForInsight` runbook to resolve OpsItems associated with an insight\. After the runbook finishes processing and all associated OpsItems are resolved, the system immediately resolves the associated insight\.

### Viewing OpsCenter operational insights<a name="OpsCenter-working-deduplication-insights-view"></a>

Use the following procedure to view operational insights about duplicate OpsItems in Systems Manager OpsCenter\.

**To view operational insights**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. On the **Overview** tab, scroll down to **Operational insights**\.

1. Choose **Enable**\.

1. To view a filtered list of insights, choose the link beside **Duplicate OpsItems** or the link beside **Sources generating most OpsItems**\. To view all insights, choose **View all operational insights**\.

1. Choose an insight ID to view more information\.

## Resolving duplicate OpsItems based on insights<a name="OpsCenter-working-deduplication-insights-resolve"></a>

Systems Manager provides the following automation runbooks to help you resolve duplicate OpsItems and reduce the number of OpsItems created by a source\.
+ The `AWS-BulkResolveOpsItems` runbook resolves OpsItems that match a specified filter\.
+ The `AWS-AddOpsItemDedupStringToEventBridgeRule` runbook adds a deduplication \(dedup\) string for all OpsItem targets associated with a given Amazon EventBridge rule\. This document doesn't add a dedup string if a rule already has one\.
+ The `AWS-DisableEventBridgeRule` disables a rule in EventBridge\. If a rule is generating dozens or hundreds of OpsItems, you might find it useful to simply disable the rule\.

To run one of these runbooks, open an insight, choose the runbook, and then choose **Execute**\. 

## Disabling operational insights<a name="OpsCenter-working-deduplication-insights-disable"></a>

You can disable operational insights on the OpsCenter **Configure sources** page\. When you disable this feature, the system stops creating new insights and stops displaying insights in the console\. Any active insights remain unchanged in the system, though you won't see them displayed in the console\. If you enable this feature again, the system displays any previously unresolved insights and starts creating new insights\. Use the following procedure to disable operational insights\.

**To disable operational insights**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose the **OpsItems** tab\.

1. Choose **Configure sources**\.

1. In the **Operational insights** section, choose **Edit** and then toggle the **Disable** option\.

1. Choose **Save**\.