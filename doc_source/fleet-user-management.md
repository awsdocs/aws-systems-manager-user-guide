# User management<a name="fleet-user-management"></a>

You can use Fleet Manager, a capability of AWS Systems Manager, to manage operating system \(OS\) user accounts on your instances\. For example, you can create and delete users and groups\. Additionally, you can view details like group membership, user roles, and status\.

**Important**  
Fleet Manager uses Run Command and Session Manager, capabilities of AWS Systems Manager, for various user management operations\. As a result, a user could grant permissions to an operating system user account that they would otherwise be unable to\. This is because AWS Systems Manager Agent \(SSM Agent\) runs on Amazon Elastic Compute Cloud \(Amazon EC2\) instances using root permissions \(Linux\) or SYSTEM permissions \(Windows Server\)\. For more information about restricting access to root\-level commands through the SSM Agent, see [Restricting access to root\-level commands through SSM Agent](ssm-agent-restrict-root-level-commands.md)\. To restrict access to this feature, we recommend creating AWS Identity and Access Management \(IAM\) policies for your users that only allow access to the actions you define\. For more information about creating IAM policies for Fleet Manager, see [Step 1: Create an IAM policy with Fleet Manager permissions](fleet-setup-iam.md)\.

## Create a user or group<a name="fleet-user-management-create"></a>

**Note**  
Fleet Manager uses Session Manager to set passwords for new users\. The instance profile attached to your managed instances must provide permissions for Session Manager to use this feature\. For more information about adding Session Manager permissions to an instance profile, see [Adding Session Manager permissions to an existing instance profile](getting-started-add-permissions-to-existing-profile.md)\. Also, AWS Key Management Service \(AWS KMS\) encryption must be turned on in your session preferences to use Fleet Manager features\. For more information about enabling AWS KMS encryption for Session Manager, see [Enable KMS key encryption of session data \(console\)](session-preferences-enable-encryption.md)\.

**To create an OS user account with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance you want to create a new user on\.

1. Choose **View details**\.

1. In the **Tools** menu, choose **Users and groups**\.

1. Choose the **Users** tab, and then choose **Create user**\.

1. Enter a value for the **Name** of the new user\.

1. \(Recommended\) Select the check box next to **Set password**\. You will be prompted to provide a password for the new user at the end of the procedure\.

1. Select **Create user**\. If you selected the check box to create a password for the new user, you will be prompted to enter a value for the password and select **Done**\. If the password you specify doesn't meet the requirements specified by your instance's local or domain policies, an error is returned\.

**To create an OS group with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance you want to create a group in\.

1. Choose **View details**\.

1. In the **Tools** menu, choose **Users and groups**\.

1. Choose the **Groups** tab, and then choose **Create group**\.

1. Enter a value for the **Name** of the new group\.

1. \(Optional\) Enter a value for the **Description** of the new group\.

1. \(Optional\) Select users to add to the **Group members** for the new group\.

1. Select **Create group**\.

## Update user or group membership<a name="fleet-user-management-update"></a>

**To add an OS user account to a new group with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance where the user account exists that you want to update\.

1. Choose **View details**\.

1. In the **Tools** menu, choose **Users and groups**\.

1. Choose the **Users** tab\.

1. Choose the button next to the user you want to update\.

1. In the **Actions** menu, choose **Add user to group**\.

1. Choose the group you want to add the user to under **Add to group**\.

1. Select **Add user to group**\.

**To edit an OS group's membership with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance where the group exists that you want to update\.

1. Choose **View details**\.

1. In the **Tools** menu, choose **Users and groups**\.

1. Choose the **Groups** tab\.

1. Choose the button next to the group you want to update\.

1. In the **Actions** menu, choose **Modify group**\.

1. Choose the users you want to add or remove under **Group members**\.

1. Select **Modify group**\.

## Delete a user or group<a name="fleet-user-management-delete"></a>

**To delete an OS user account with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance where the user account exists that you want to delete\.

1. Choose **View details**\.

1. In the **Tools** menu, choose **Users and groups**\.

1. Choose the **Users** tab\.

1. Choose the button next to the user you want to delete\.

1. In the **Actions** menu, choose **Delete local user**\.

**To delete an OS group with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance where the group exists that you want to delete\.

1. Choose **View details**\.

1. In the **Tools** menu, choose **Users and groups**\.

1. Choose the **Group** tab\.

1. Choose the button next to the group you want to update\.

1. In the **Actions** menu, choose **Delete local group**\.