# Viewing the file system<a name="fleet-file-management"></a>

You can use Fleet Manager, a capability of AWS Systems Manager, to view information about the folder and file data stored on the volumes attached to your managed instances\. For example, you can view the name, size, extension, owner, and permissions for your folders and files\. Up to 10,000 lines of file data can be previewed as text from the Fleet Manager console\. You can also use this feature to `tail` files\. When using `tail` to view file data, the last 10 lines of the file are displayed initially\. As new lines of data are written to the file, the view is updated in real\-time\. As a result, you can review log data from the console, which can improve the efficiency of your troubleshooting and systems administration\.

**Note**  
Fleet Manager uses Session Manager, a capability of AWS Systems Manager, to view text previews and `tail` files\. The instance profile attached to your managed instances must provide permissions for Session Manager to use this feature\. For more information about adding Session Manager permissions to an instance profile, see [Adding Session Manager permissions to an existing instance profile](getting-started-add-permissions-to-existing-profile.md)\. Also, AWS Key Management Service \(AWS KMS\) encryption must be turned on in your session preferences to use Fleet Manager features\. For more information about enabling AWS KMS encryption for Session Manager, see [Turn on KMS key encryption of session data \(console\)](session-preferences-enable-encryption.md)\.

**To view the file system with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance whose file system you want to view\.

1. Choose **View details**\.

1. In the **Tools** menu, choose **File system**\.

**To view text previews of files with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance whose files you want to preview\.

1. Choose **View details**\.

1. In the **Tools** menu, choose **File system**\.

1. Select the **File name** of the folder that contains the file you want to preview\.

1. Choose the button next to the file whose content you want to preview\.

1. In the **Actions** menu, choose **View text preview**\.

**To tail files with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance whose files you want to tail\.

1. Choose **View details**\.

1. In the **Tools** menu, choose **File system**\.

1. Select the **File name** of the folder that contains the file you want to preview\.

1. Choose the button next to the file whose content you want to tail\.

1. In the **Actions** menu, choose **View with tail**\.