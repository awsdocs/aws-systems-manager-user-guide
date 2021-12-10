# Working with the file system<a name="fleet-file-management"></a>

You can use Fleet Manager, a capability of AWS Systems Manager, to work with the file system on your managed nodes\. Using Fleet Manager, you can view information about the directory and file data stored on the volumes attached to your managed nodes\. For example, you can view the name, size, extension, owner, and permissions for your directories and files\. Up to 10,000 lines of file data can be previewed as text from the Fleet Manager console\. You can also use this feature to `tail` files\. When using `tail` to view file data, the last 10 lines of the file are displayed initially\. As new lines of data are written to the file, the view is updated in real time\. As a result, you can review log data from the console, which can improve the efficiency of your troubleshooting and systems administration\. Additionally, you can create directories and copy, cut, paste, rename, or delete files and directories\.

We recommend creating regular backups, or taking snapshots of the Amazon Elastic Block Store \(Amazon EBS\) volumes attached to your managed nodes\. When copying, or cutting and pasting files, existing files and directories in the destination path with the same name as the new files or directories are replaced\. Serious problems can occur if you replace or modify system files and directories\. AWS doesn't guarantee that these problems can be solved\. Modify system files at your own risk\. You're responsible for all file and directory changes, and ensuring you have backups\. Deleting or replacing files and directories can't be undone\.

**Note**  
Fleet Manager uses Session Manager, a capability of AWS Systems Manager, to view text previews and `tail` files\. For Amazon Elastic Compute Cloud \(Amazon EC2\) instances, the instance profile attached to your managed instances must provide permissions for Session Manager to use this feature\. For more information about adding Session Manager permissions to an instance profile, see [Adding Session Manager permissions to an existing IAM role](getting-started-add-permissions-to-existing-profile.md)\. Also, AWS Key Management Service \(AWS KMS\) encryption must be turned on in your session preferences to use Fleet Manager features\. For more information about enabling AWS KMS encryption for Session Manager, see [Turn on KMS key encryption of session data \(console\)](session-preferences-enable-encryption.md)\.

**To view the file system with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Select the link of the managed node with the file system you want to view\.

1. In the **Tools** menu, choose **File system**\.

**To view text previews of files with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Select the link of the managed node with the files you want to preview\.

1. In the **Tools** menu, choose **File system**\.

1. Select the **File name** of the directory that contains the file you want to preview\.

1. Choose the button next to the file whose content you want to preview\.

1. In the **Actions** menu, choose **View text preview**\.

**To tail files with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Select the link of the managed node with the files you want to tail\.

1. In the **Tools** menu, choose **File system**\.

1. Select the **File name** of the directory that contains the file you want to tail\.

1. Choose the button next to the file whose content you want to tail\.

1. In the **Actions** menu, choose **View with tail**\.

**To copy or cut and paste files or directories with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Select the link of the managed node with the files you want to copy, or cut and paste\.

1. In the **Tools** menu, choose **File system**\.

1. To copy or cut a file, select the **File name** of the directory that contains the file you want to copy or cut\. To copy or cut a directory, choose the button next to the directory that you want to copy or cut and then proceed to step 8\.

1. Choose the button next to the file you want to copy or cut\.

1. In the **Actions** menu, choose **Copy** or **Cut**\.

1. In the **File system** view, choose the button next to the directory you want to paste the file in\.

1. In the **Actions** menu, choose **Paste**\.

**To rename files or directories with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Select the link of the managed node with the files or directories you want to rename\.

1. In the **Tools** menu, choose **File system**\.

1. To rename a file, select the **File name** of the directory that contains the file you want to rename\. To rename a directory, choose the button next to the directory that you want to rename and then proceed to step 8\.

1. Choose the button next to the file whose content you want to rename\.

1. In the **Actions** menu, choose **Rename**\.

1. In the **File name** field, enter the new name for the file and select **Rename**\.

**To delete files or directories with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Select the link of the managed node with the files or directories you want to delete\.

1. In the **Tools** menu, choose **File system**\.

1. To delete a file, select the **File name** of the directory that contains the file you want to delete\. To delete a directory, choose the button next to the directory that you want to delete and then proceed to step 7\.

1. Choose the button next to the file with the content you want to delete\.

1. In the **Actions** menu, choose **Delete**\.

**To create a directory with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Select the link of the managed node you want to create a directory in\.

1. In the **Tools** menu, choose **File system**\.

1. Select the **File name** of the directory where you want to create a new directory\.

1. Select **Create directory**\.

1. In the **Directory name** field, enter the name for the new directory and select **Create directory**\.