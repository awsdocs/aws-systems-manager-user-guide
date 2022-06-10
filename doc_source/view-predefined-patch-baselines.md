# Viewing AWS predefined patch baselines \(console\)<a name="view-predefined-patch-baselines"></a>

Patch Manager, a capability of AWS Systems Manager, includes a predefined patch baseline for each operating system supported by Patch Manager\. You can use these patch baselines \(you can't customize them\), or you can create your own\. The following procedure describes how to view a predefined patch baseline to see if it meets your needs\. To learn more about patch baselines, see [About predefined and custom patch baselines](sysman-patch-baselines.md)\.

**To view AWS predefined patch baselines**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. In the patch baselines list, choose the baseline ID of one of the predefined patch baselines\.

   \-or\-

   If you are accessing Patch Manager for the first time in the current AWS Region, choose **View predefined patch baselines**, and then choose the baseline ID of one of the predefined patch baselines\.
**Note**  
For Windows Server, three predefined patch baselines are provided\. The patch baselines `AWS-DefaultPatchBaseline` and `AWS-WindowsPredefinedPatchBaseline-OS` support only operating system updates on the Windows operating system itself\. `AWS-DefaultPatchBaseline` is used as the default patch baseline for Windows Server managed nodes unless you specify a different patch baseline\. The configuration settings in these two patch baselines are the same\. The newer of the two, `AWS-WindowsPredefinedPatchBaseline-OS`, was created to distinguish it from the third predefined patch baseline for Windows Server\. That patch baseline, `AWS-WindowsPredefinedPatchBaseline-OS-Applications`, can be used to apply patches to both the Windows Server operating system and supported applications released by Microsoft\.  
For more information, see [Setting an existing patch baseline as the default \(console\)](set-default-patch-baseline.md)\.

1. Choose the **Approval rules** tab and review the patch baseline configuration\.

1. If the configuration is acceptable for your managed nodes, you can skip ahead to the procedure [Working with patch groups](sysman-patch-group-tagging.md)\. 

   \-or\-

   To create your own default patch baseline, continue to the topic [Working with custom patch baselines \(console\)](sysman-patch-baseline-console.md)\.