# Tagging maintenance windows<a name="tagging-maintenance-windows"></a>

The topics in this section describe how to work with tags on maintenance windows\.

**Topics**
+ [Creating maintenance windows with tags](#tagging-maintenance-windows-new)
+ [Adding tags to existing maintenance windows](#tagging-maintenance-windows-update)
+ [Removing tags from maintenance windows](#tagging-maintenance-windows-remove)

## Creating maintenance windows with tags<a name="tagging-maintenance-windows-new"></a>

You can add tags to maintenance windows at the time you create them\.

For information, see the following topics:
+ [Create a maintenance window \(console\)](sysman-maintenance-create-mw.md)
+ [Tutorial: Create and configure a maintenance window \(AWS CLI\)](maintenance-windows-cli-tutorials-create.md)Create and configure a maintenance window \(AWS CLI\)

## Adding tags to existing maintenance windows<a name="tagging-maintenance-windows-update"></a>

You can add tags to maintenance windows that you own by using the Systems Manager console or the command line\.

**Topics**
+ [Adding tags to an existing maintenance window \(console\)](#tagging-maintenance-windows-update-console)
+ [Adding tags to an existing maintenance window \(AWS CLI\)](#tagging-maintenance-windows-update-command-line)
+ [Tag a maintenance window \(AWS Tools for PowerShell\)](#tagging-maintenance-windows-update-ps)

### Adding tags to an existing maintenance window \(console\)<a name="tagging-maintenance-windows-update-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Maintenance Windows**\.

1. Choose the name of a maintenance window you have already created, and then choose the **Tags** tabs\.

1. Choose **Edit tags**, and then choose **Add tag**\.

1. For **Key**, enter a key for the tag, such as **Environment**\.

1. For **Value**, enter a value for the tag, such as **Test**\.

1. Choose **Save changes**\.

### Adding tags to an existing maintenance window \(AWS CLI\)<a name="tagging-maintenance-windows-update-command-line"></a>

1. Using your preferred command line tool, run the following command to view the list of maintenance windows that you can tag\.

   ```
   aws ssm describe-maintenance-windows
   ```

   Note the ID of a maintenance window that you want to tag\.

1. Run the following command to tag a maintenance window\.

------
#### [ Linux ]

   ```
   aws ssm add-tags-to-resource \
       --resource-type "MaintenanceWindow" \
       --resource-id "window-id" \
       --tags "Key=tag-key,Value=tag-value"
   ```

------
#### [ Windows ]

   ```
   aws ssm add-tags-to-resource ^
       --resource-type "MaintenanceWindow" ^
       --resource-id "window-id" ^
       --tags "Key=tag-key,Value=tag-value"
   ```

------

   If successful, the command has no output\.

   *windows\-id* is the ID of the maintenance window you want to tag, such as mw\-0c50858d01EXAMPLE\.

   *tag\-key* is the name of a custom key you supply\. For example, *Environment* or *Project*\.

   *tag\-value* is the custom content for the value you want to supply for that key\. For example, *Production* or *Q321*\.

1. Run the following command to verify the maintenance window tags\.

------
#### [ Linux ]

   ```
   aws ssm list-tags-for-resource \
       --resource-type "MaintenanceWindow" \
       --resource-id "window-id"
   ```

------
#### [ Windows ]

   ```
   aws ssm list-tags-for-resource ^
       --resource-type "MaintenanceWindow" ^
       --resource-id "window-id"
   ```

------

### Tag a maintenance window \(AWS Tools for PowerShell\)<a name="tagging-maintenance-windows-update-ps"></a>

1. Run the following command to list maintenance windows that you can tag\.

   ```
   Get-SSMMaintenanceWindow
   ```

1. Run the following commands to tag a maintenance window\.

   ```
   $tag = New-Object Amazon.SimpleSystemsManagement.Model.Tag
   ```

   ```
   $tag.Key = "tag-key"
   ```

   ```
   $tag.Value = "tag-value"
   ```

   ```
   Add-SSMResourceTag `
       -ResourceType "MaintenanceWindow" `
       -ResourceId "window-id" `
       -Tag $tag
   ```

   *window\-id* the ID of the maintenance window you want to tag\.

   *tag\-key* is the name of a custom key you supply\. For example, *Environment* or *Project*\.

   *tag\-value* is the custom content for the value you want to supply for that key\. For example, *Production* or *Q321*\.

1. Run the following command to verify the maintenance window tags\.

   ```
   Get-SSMResourceTag `
       -ResourceType "MaintenanceWindow" `
       -ResourceId "window-id"
   ```

## Removing tags from maintenance windows<a name="tagging-maintenance-windows-remove"></a>

You can use the Systems Manager console or the command line to remove tags from maintenance windows\.

**Topics**
+ [Removing tags from maintenance windows \(console\)](#tagging-maintenance-windows-remove-console)
+ [Removing tags from maintenance windows \(command line\)](#tagging-maintenance-windows-remove-command-line)

### Removing tags from maintenance windows \(console\)<a name="tagging-maintenance-windows-remove-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Maintenance Windows**\.

1. Choose the name of the maintenance window to remove tags from, and then choose **Tags** tab\.

1. Choose **Edit tags**, and then choose **Remove tag** next to the tag pair you no longer need\.

1. Choose **Save changes**\.

### Removing tags from maintenance windows \(command line\)<a name="tagging-maintenance-windows-remove-command-line"></a>

1. Using your preferred command line tool, run the following command to list the maintenance windows in your account\.

------
#### [ Linux ]

   ```
   aws ssm describe-maintenance-windows
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-maintenance-windows
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMMaintenanceWindows
   ```

------

   Note the ID of a maintenance window from which you want to remove tags\.

1. Run the following command to remove tags from a maintenance window\.

------
#### [ Linux ]

   ```
   aws ssm remove-tags-from-resource \
       --resource-type "MaintenanceWindow" \
       --resource-id "window-id" \
       --tag-key "tag-key"
   ```

------
#### [ Windows ]

   ```
   aws ssm remove-tags-from-resource ^
       --resource-type "MaintenanceWindow" ^
       --resource-id "window-id" ^
       --tag-key "tag-key"
   ```

------
#### [ PowerShell ]

   ```
   Remove-SSMResourceTag `
       -ResourceType "MaintenanceWindow" `
       -ResourceId "window-id" `
       -TagKey "tag-key
   ```

------

   *windows\-id* is the ID of the maintenance window from which you want to remove a tag, such as mw\-0c50858d01EXAMPLE\.

   *tag\-key* is the name of a key assigned to the maintenance window\. For example, *Environment* or *Quarter*\.

   If successful, the command has no output\.

1. Run the following command to verify the maintenance window tags\.

------
#### [ Linux ]

   ```
   aws ssm list-tags-for-resource \
       --resource-type "MaintenanceWindow" \
       --resource-id "window-id"
   ```

------
#### [ Windows ]

   ```
   aws ssm list-tags-for-resource ^
       --resource-type "MaintenanceWindow" ^
       --resource-id "window-id"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMResourceTag `
       -ResourceType "MaintenanceWindow" `
       -ResourceId "window-id"
   ```

------