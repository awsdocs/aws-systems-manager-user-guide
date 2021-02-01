# Updating or deleting a custom patch baseline \(console\)<a name="patch-baseline-update-or-delete"></a>

You can update or delete a custom patch baseline that you have created\. When you update a patch baseline, you can change its name or description, its approval rules, and its exceptions for approved and rejected patches\. You can also update the tags that are applied to the patch baseline\. You can't change the operating system type that a patch baseline has been created for, and you can't make changes to a predefined patch baseline provided by AWS\.

## Updating or deleting a patch baseline \(console\)<a name="sysman-maintenance-update-mw"></a>

Follow these steps to update or delete a patch baseline\.

**To update or delete a patch baseline \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose the patch baseline that you want to update or delete, and then do one of the following:
   + To remove the patch baseline from your account, choose **Delete**\. The system prompts you to confirm your actions\. 
   + To make changes to the patch baseline name or description, approval rules, or patch exceptions, choose **Edit**\. On the **Edit patch baseline** page, change the values and options that you want, and then choose **Save changes**\. 
   + To add, change, or delete tags applied to the patch baseline, choose the **Tags** tab, and then choose **Edit tags**\. On the **Edit patch baseline tags** page, make updates to the patch baseline tags, and then choose **Save changes**\. 

   For information about the configuration choices you can make, see [Working with custom patch baselines](sysman-patch-baseline-console.md)\.