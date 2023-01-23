# About approvals in your change templates<a name="cm-approvals-templates"></a>

For each change template that you create, you can specify up to five approval *levels* for change requests created from it\. For each of those levels, you can designate up to five potential *approvers*\. An approver isn't limited to a single AWS Identity and Access Management \(IAM\) user\. You can also specify an IAM group or IAM role as an individual approver\. For IAM groups and IAM roles, one or more users belonging to the group or role can provide approvals toward receiving the total number of approvals required for a change request\. You can also specify more approvers than your change template requires\.

Change Manager supports two main approaches to approvals: *per\-level approvals* and *per\-line approvals*\. A combination of the two types is also possible in some situations\. We recommend using only per\-level approvals in your Change Manager operations\.

------
#### [ Per\-level approvals ]

*Recommended*\. As of January 23, 2023, Change Manager supports per\-level approvals\. In this model, for each approval level in your change template, you first specify how many approvals are required for that level\. Then you specify at least that many approvers for the level and can specify more approvers\. However, only the number of per\-level approvers that you specify need to approve the change request\. For example, you could specify five approvers but require three approvals\.

For console\-view and JSON samples of this approval type, see [Sample per\-level approval configuration](approval-type-samples.md#per-level-approvals)\.

------
#### [ Per\-line approvals ]

*Supported for backward compatibility*\. The original release of Change Manager supported only per\-line approvals\. In this model, every approver specified for an approval level is represented as an approval line\. Each approver had to approve a change request for it to be approved at that level\. Prior to January 23, 2023, this was the only supported model for approvals\. Change templates created before this date continue to support per\-line approvals, but we recommend using per\-level approvals instead\.

For console\-view and JSON samples of this approval type, see [Sample per\-line approval configuration](approval-type-samples.md#per-line-approvals)\.

------
#### [ Combined per\-line and per\-level approvals ]

*Not recommended*\. In the console, the **Builder** tab no longer supports adding per\-line approvals\. However, in some cases you might end up with both per\-line and per\-level approvals in a change template\. This can occur if you update a change template that was created before January 23, 2023, or if you create or update a change template by editing its YAML content manually,

For console\-view and JSON samples of this approval type, see [Sample combined per\-level and per\-line approval configuration](approval-type-samples.md#combined-approval-levels)\.

------

**Important**  
Although it's possible to create a change template that combines per\-line and per\-level approvals, this configuration isn't recommended or necessary\. Whichever approval type requires more approvals \(per\-line or per\-level approvals\) takes precedence\. For example:  
If a change template specifies three per\-level approvals but five per\-line approvals, then five approvals are required\.
If a change template specifies four per\-level approvals but two per\-line approvals, then four approvals are required\.

You can create a level that includes both per\-line and per\-level approvals by editing the YAML or JSON content manually\. Then, the **Builder** tab displays controls for specifying the required number of approvals for both the level and for individual lines\. However, new levels that you add using the console still support only per\-level approval configurations\.

## Change request notifications and rejections<a name="notifications-and-rejections"></a>

Amazon SNS notifications  
When a change request is created using your change template, notifications are sent to subscribers of the Amazon Simple Notification Service \(Amazon SNS\) topic that has been designated for approval notifications at that level\. You can specify the notification topic in the change template or allow the user creating the change request to specify one\.  
After the minimum number of required approvals is received at one level, notifications are sent to approvers subscribed to the Amazon SNS topic for the next level, and so on\.  
Ensure that the IAM roles, groups, and users you designate together provide enough approvers to meet the required number of approvals you specify\. For example, if you designate only a single IAM group as an approver that contains three users, you can't specify that five approvals are mandatory at that level, only three or less\.

Change request rejections  
No matter how many approval levels and approvers you specify, only one rejection to a change request is required to prevent the runbook workflow for that request from occurring\.