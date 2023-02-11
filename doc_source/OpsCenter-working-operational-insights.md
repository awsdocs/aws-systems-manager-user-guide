# Analyzing operational insights to reduce duplicate OpsItems<a name="OpsCenter-working-operational-insights"></a>

OpsCenter includes the *Operational insights* feature, which display information about duplicate OpsItems\. OpsCenter automatically analyzes OpsItems in your account and generates two types of *insights* in the **Operational insights** section: 
+ **Duplicate OpsItems** – This field displays a count of insights when eight or more OpsItems have the same title for the same resource\.
+ **Sources generating most OpsItems** – This field displays a count of insights when more than 50 OpsItems have the same title\.

When you choose an insight, OpsCenter displays information about the affected OpsItems and resources\. The following screenshot shows an example with the details of a duplicate OpsItem insight\. 

![\[Detailed view of an OpsCenter insight with information about duplicate OpsItems.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsCenter-insights-detailed.png)

**Note**  
Operational insights are turned off by default\. For information about enabling operational insights, see [Analyzing operational insights](#OpsCenter-working-operational-insights-viewing)\. 

For more information about working with operational insights, see the following topics\.

**Topics**
+ [Analyzing operational insights](#OpsCenter-working-operational-insights-viewing)
+ [Resolving duplicate OpsItems based on insights](#OpsCenter-working-operational-insights-resolve)
+ [Turning off operational insights](#OpsCenter-working-operational-insights-disable)

## Analyzing operational insights<a name="OpsCenter-working-operational-insights-viewing"></a>

When you enable operational insights, Systems Manager creates an AWS Identity and Access Management \(IAM\) service\-linked role called `AWSServiceRoleForAmazonSSM_OpsInsights`\. A service\-linked role is a unique type of IAM role that is linked directly to Systems Manager\. Service\-linked roles are predefined and include all the permissions that the service requires to call other AWS services on your behalf\. For more information about the `AWSServiceRoleForAmazonSSM_OpsInsights` service\-linked role, see [Using roles to create operational insight OpsItems in Systems Manager OpsCenter: `AWSServiceRoleForAmazonSSM_OpsInsights`](using-service-linked-roles-service-action-4.md)\.

 OpsCenter creates OpsItems of type `insight`\. OpsCenter has a limit of 25 insights\. The system stops creating new insights when it reaches this limit\. Your AWS account is charged for insight OpsItems created by the system\. For more information, see [AWS Systems Manager Pricing](http://aws.amazon.com/systems-manager/pricing/)\.

**Note**  
OpsCenter periodically refreshes insights using a batch process\. This means the list of insights displayed in OpsCenter might be out of sync\.

Use the following procedure to view operational insights about duplicate OpsItems in Systems Manager OpsCenter\.

**To enable and view operational insights**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. On the **Overview** tab, scroll down to **Operational insights**\.

1. Choose **Enable**\.

1. To view a filtered list of insights, choose the link beside **Duplicate OpsItems** or the link beside **Sources generating most OpsItems**\. To view all insights, choose **View all operational insights**\.

1. Choose an insight ID to view more information\.

## Resolving duplicate OpsItems based on insights<a name="OpsCenter-working-operational-insights-resolve"></a>

To resolve insights, you must first resolve all OpsItems associated with an insight\. You can use the `AWS-BulkResolveOpsItemsForInsight` runbook to resolve OpsItems associated with an insight\. 

To help you resolve duplicate OpsItems and reduce the number of OpsItems created by a source, Systems Manager provides the following automation runbooks:
+ The `AWS-BulkResolveOpsItems` runbook resolves OpsItems that match a specified filter\.
+ The `AWS-AddOpsItemDedupStringToEventBridgeRule` runbook adds a deduplication string for all OpsItem targets that are associated with a specific Amazon EventBridge rule\. This runbook doesn't add a deduplication string if a rule already has one\.
+ The `AWS-DisableEventBridgeRule` turns off a rule in EventBridge if the rule is generating dozens or hundreds of OpsItems\.

To run one of these runbooks, open an insight, choose the runbook, and then choose **Execute**\. 

## Turning off operational insights<a name="OpsCenter-working-operational-insights-disable"></a>

When you turn off operational insights, the system stops creating new insights and stops displaying insights in the console\. Any active insights remain unchanged in the system, although you won't see them displayed in the console\. If you enable this feature again, the system displays previously unresolved insights and starts creating new insights\. Use the following procedure to turn off operational insights\.

**To turn off operational insights**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose the **OpsItems** tab\.

1. Choose **Configure sources**\.

1. In the **Operational insights** section, choose **Edit** and then toggle the **Disable** option\.

1. Choose **Save**\.