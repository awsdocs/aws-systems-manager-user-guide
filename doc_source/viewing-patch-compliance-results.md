# Viewing patch compliance results<a name="viewing-patch-compliance-results"></a>

Use this procedure to view patch compliance information about your managed instances\.

This procedure applies to patch operations that use the `AWS-RunPatchBaseline` document\. For information about viewing patch compliance information for patch operations that use the `AWS-RunPatchBaselineAssociation` document, see [Identifying out\-of\-compliance instances](patch-compliance-identify.md)\.

**Note**  
The AWS Systems Manager Quick Setup and AWS Systems ManagerExplorer patch scan processes use the `AWS-RunPatchBaselineAssociation` document\.

**To view patch compliance results**

1. Do one of the following\.

   **Option 1** – Navigate from **Compliance**:
   + In the navigation pane, choose **Compliance**\.

     \-or\-

     If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Compliance** in the navigation pane\.
   + For **Compliance resources summary**, choose a number in the column for the types of patch resources you want to review, such as **Non\-Compliant resources**\.
   + In the **Resource** list, choose the ID of the instance for which you want to review patch compliance results\.

   **Option 2** – Navigate from **Managed Instances**\.
   + In the navigation pane, choose **Managed Instances**\.

     \-or\-

     If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Managed Instances**\.
   + On the **Managed Instances** tab, choose the ID of the instance for which you want to review patch compliance results\.

1. Choose the **Patch** tab\.

1. \(Optional\) In the Search box \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/search-icon.png)\), choose from the following filters:
   + Classification
   + KB
   + Severity
   + State

1. Choose one of the available values for the filter type you chose\. For example, if you chose **State**, now choose a compliance state such as **Failed** or **Missing**\.

1. Depending on the compliance state of the instance, you can choose what action to take to remedy any noncompliant instances\.

   For example, you can choose to patch your noncompliant instances immediately\. For information about patching your instances on demand, see [Patching instances on demand](patch-on-demand.md)\.

   For information about patch compliance states, see [Understanding patch compliance state values](about-patch-compliance-states.md)\.