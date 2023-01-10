# Working with OpsItems across accounts<a name="OpsCenter-working-with-multiple-accounts"></a>

OpsCenter supports working with OpsItems from a management or delegated administrator account and a member account during a session\. Once configured, users can perform the following types of actions:
+ Create, view, and update OpsItems in a member account\.
+ View detailed information about AWS resources specified in OpsItems in another account\.
+ Start Systems Manager Automation runbooks to remediate issues with AWS resources in a member account\.

**Note**  
OpsCenter and Explorer, a capability of Systems Manager, share an integrated setup experience\. If you haven't already done so while setting up Explorer, we recommend that you create a delegated administrator account\. Specifying one AWS account as a delegated administrator can improve security for several administrator tasks, including working with OpsItems across accounts\. For more information, see [Configuring a delegated administrator](Explorer-setup-delegated-administrator.md)\.

**Before you begin**  
Verify that you've configured OpsCenter for working with OpsItems across accounts\. For more information, see [\(Optional\) Setting up OpsCenter to work with OpsItems across accounts](OpsCenter-getting-started-multiple-accounts.md)\.

**Topics**
+ [Creating OpsItems in a member account](#OpsCenter-working-with-multiple-accounts-creating)
+ [Working with OpsItems across accounts](#OpsCenter-working-with-multiple-accounts-working-with)

## Creating OpsItems in a member account<a name="OpsCenter-working-with-multiple-accounts-creating"></a>

If you're signed in to an AWS Organizations management account or the Systems Manager delegated administrator account you can create OpsItems in other accounts manually\. On the **Create OpsItem** page, you choose the **Other account** option\. After you create the OpsItem, you can expand the **OpsItem details** and view the account ID to which you assigned the OpsItem, as shown in the following image\.

![\[The management account view for an OpsItem assigned to a separate account.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsCenter-create-multi-2.png)

For information about manually creating OpsItems, see [Creating OpsItems manually](OpsCenter-manually-create-OpsItems.md)\.

**Note**  
If you manually create OpsItems by using the AWS CLI or AWS Tools for PowerShell, you must specify the `account-id` parameter to create an OpsItem in a separate account\.

## Working with OpsItems across accounts<a name="OpsCenter-working-with-multiple-accounts-working-with"></a>

If your account has permission, you can also view and update OpsItems in a member account during a session\. This includes interacting with AWS resources on the OpsItem **Related resource details** tab\. You can also choose and run Automation runbooks against resources in a separate account\.

Perform the following steps while signed in to an AWS Organizations management account, a delegated administrator account, or an account with the required permissions and policies\. 

**To work with OpsItems across accounts**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose **OpsItems**\.

1. Choose the Search bar\.

1. Choose **Account ID** from the filter list\.

1. Specify an account ID, and press Enter\.

The OpsItem list displays OpsItems from the specified account and the account you are currently signed into\. The list displays the **Account ID** column\.