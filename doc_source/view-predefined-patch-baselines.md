# Viewing AWS predefined patch baselines<a name="view-predefined-patch-baselines"></a>

Patch Manager includes a predefined patch baseline for each operating system supported by Patch Manager\. You can use these patch baselines \(you can't customize them\), or you can create your own\. The following procedure describes how to view a predefined patch baseline to see if it meets your needs\. To learn more about patch baselines, see [About predefined and custom patch baselines](sysman-patch-baselines.md)\.

**To view AWS predefined patch baselines**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. In the patch baselines list, choose the baseline ID of one of the predefined patch baselines\.
**Note**  
For Windows Server, two predefined patch baselines are provided\. The patch baseline `AWS-WindowsPredefinedPatchBaseline-OS` supports only operating system updates on the Windows operating system itself\. It is used as the default patch baseline for Windows Server instances unless you specify a different patch baseline\. The other predefined Windows patch baseline, `AWS-WindowsPredefinedPatchBaseline-OS-Applications`, can be used to apply patches to both the Windows Server operating system and supported Microsoft applications\.   
For more information, see [ Setting an existing patch baseline as the default](set-default-patch-baseline.md)\.

1. Choose the **Approval rules** tab and review the patch baseline configuration\.

1. If the configuration is acceptable for your instances, you can skip ahead to the procedure [Creating a patch group](sysman-patch-group-tagging.md)\. 

   \-or\-

   To create your own default patch baseline, continue to the topic [Working with custom patch baselines](sysman-patch-baseline-console.md)\.