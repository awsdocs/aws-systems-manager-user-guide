# Creating OpsItems manually \(console\)<a name="OpsCenter-creating-OpsItems-console"></a>

 You can manually create OpsItems using the AWS Systems Manager console\. When you create an OpsItem, it's displayed in your OpsCenter account\. If you set up OpsCenter for cross\-account administration, OpsCenter provides the delegated administrator or management account with the option to create OpsItems for selected member accounts\. For more information, see [\(Optional\) Setting up OpsCenter to centrally manage OpsItems across accounts](OpsCenter-getting-started-multiple-accounts.md)\.

**To create an OpsItem using the AWS Systems Manager console**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose **Create OpsItem**\. If you don't see this button, choose the **OpsItems** tab, and then choose **Create OpsItem**\.

1.  \(Optional\) Choose **Other account**, and then choose the account where you want to create the OpsItem\. 
**Note**  
This step is required if you're creating OpsItems for a member account\. 

1. For **Title**, enter a descriptive name to help you understand the purpose of the OpsItem\.

1. For **Source**, enter the type of impacted AWS resource or other source information to help users understand the origin of the OpsItem\.
**Note**  
You can't edit the **Source** field after you create the OpsItem\.

1. \(Optional\) For **Priority**, choose the priority level\.

1. \(Optional\) For **Severity**, choose the severity level\.

1. \(Optional\) For **Category**, choose a category\.

1. For **Description**, enter information about this OpsItem including \(if applicable\) steps for reproducing the issue\. 

1. For **Deduplication string**, enter words that the system can use to check for duplicate OpsItems\. For more information about deduplication strings, see [Managing duplicate OpsItems](OpsCenter-working-deduplication.md)\. 

1. \(Optional\) For **Notifications**, specify the Amazon Resource Name \(ARN\) of the Amazon SNS topic where you want notifications sent when this OpsItem is updated\. You must specify an Amazon SNS ARN that is in the same AWS Region as the OpsItem\.

1. \(Optional\) For **Related resources**, choose **Add** to specify the ID or ARN of the impacted resource and any related resources\.

1. Choose **Create OpsItem**\.

If successful, the page displays the OpsItem\. When a delegated administrator or management account creates an OpsItem for selected member accounts, the new OpsItems are displayed in the OpsCenter of the administrator and members accounts\. For information about how to configure the options in an OpsItem, see [Manage OpsItems](OpsCenter-working-with-OpsItems.md)\.