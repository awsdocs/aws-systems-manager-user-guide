# Windows registry management<a name="fleet-registry"></a>

You can use Fleet Manager to manage the registry on your Windows instances\. From the Fleet Manager console you can create, copy, update, and delete registry entries and values\.

**Important**  
We recommend creating a backup of the registry, or taking a snapshot of the root Amazon Elastic Block Store \(Amazon EBS\) volume attached to your instance before you modify the registry\. Serious problems can occur if you modify the registry incorrectly\. These problems might require you to reinstall the operating system, or restore the root volume of your instance from a snapshot\. AWS does not guarantee that these problems can be solved\. Modify the registry at your own risk\. You are responsible for all registry changes, and ensuring you have backups\.

## Create a Windows registry key or entry<a name="fleet-registry-create"></a>

**To create a Windows registry key with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance you want to create a registry key on\.

1. Choose **View details**\.

1. In the **Tools** menu, choose **Windows registry**\.

1. Choose the hive you want to create a new registry key in by selecting the **Registry name**\.

1. In the **Create** menu, choose **Create registry key**\.

1. Choose the button next to the registry entry you want to create a new key in\.

1. Choose **Create registry key**\.

1. Enter a value for the **Name** of the new registry key, and select **Submit**\.

**To create a Windows registry entry with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance you want to create a registry entry on\.

1. Choose **View details**\.

1. In the **Tools** menu, choose **Windows registry**\.

1. Choose the hive, and subsequent registry key you want to create a new registry entry in by selecting the **Registry name**\.

1. In the **Create** menu, choose **Create registry entry**\.

1. Enter a value for the **Name** of the new registry entry\.

1. Choose the **Type** of value you want to create for the registry entry\. For more information about registry value types, see [Registry value types](https://docs.microsoft.com/en-us/windows/win32/sysinfo/registry-value-types)\.

1. Enter a value for the **Value** of the new registry entry\.

## Update a Windows registry entry<a name="fleet-registry-update"></a>

**To update a Windows registry entry with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance you want to update a registry entry on\.

1. Choose **View details**\.

1. In the **Tools** menu, choose **Windows registry**\.

1. Choose the hive, and subsequent registry key you want to update by selecting the **Registry name**\.

1. Choose the button next to the registry entry you want to update\.

1. In the **Actions** menu, choose **Update registry entry**\.

1. Enter the new value for the **Value** of the registry entry\.

1. Choose **Update**\.

## Delete a Windows registry entry or key<a name="fleet-registry-delete"></a>

**To delete a Windows registry key with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance you want to delete a registry key on\.

1. In the **Tools** menu, choose **Windows registry**\.

1. Choose the hive, and subsequent registry key you want to delete by selecting the **Registry name**\.

1. Choose the button next to the registry key you want to delete\.

1. In the **Actions** menu, choose **Delete registry key**\.

**To delete a Windows registry entry with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance you want to delete a registry entry on\.

1. Choose **View details**\.

1. In the **Tools** menu, choose **Windows registry**\.

1. Choose the hive, and subsequent registry key containing the entry you want to delete by selecting the **Registry name**\.

1. Choose the button next to the registry entry you want to delete\.

1. In the **Actions** menu, choose **Delete registry entry**\.