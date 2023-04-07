# Tagging patch baselines<a name="tagging-patch-baselines"></a>

The topics in this section describe how to work with tags on patch baselines\.

**Topics**
+ [Creating patch baselines with tags](#tagging-patch-baselines-new)
+ [Adding tags to existing patch baselines](#tagging-patch-baselines-update)
+ [Removing tags from patch baselines](#tagging-patch-baselines-remove)

## Creating patch baselines with tags<a name="tagging-patch-baselines-new"></a>

You can add tags to AWS Systems Manager patch baselines at the time you create them\.

For information, see the following topics:
+ [Working with custom patch baselines \(console\)](patch-manager-manage-patch-baselines.md)
+ [Create a patch baseline](patch-manager-cli-commands.md#patch-manager-cli-commands-create-patch-baseline)
+ [Create a patch baseline with custom repositories for different OS versions](patch-manager-cli-commands.md#patch-manager-cli-commands-create-patch-baseline-mult-sources)

## Adding tags to existing patch baselines<a name="tagging-patch-baselines-update"></a>

You can add tags to patch baselines that you own by using the Systems Manager console or the command line\.

**Topics**
+ [Adding tags to an existing patch baseline \(console\)](#tagging-patch-baselines-update-console)
+ [Adding tags to an existing patch baseline \(AWS CLI\)](#tagging-patch-baselines-update-command-line)
+ [Tag a patch baseline \(AWS Tools for PowerShell\)](#tagging-patch-baselines-update-ps)

### Adding tags to an existing patch baseline \(console\)<a name="tagging-patch-baselines-update-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose the name of a custom patch baseline you have already created, scroll down to the **Tags table** section, and then choose **Edit tags**\.

1. Choose **Add tag**\.

1. For **Key**, enter a key for the tag, such as **Environment**\.

1. For **Value**, enter a value for the tag, such as **Test**\.

1. Choose **Save changes**\.

### Adding tags to an existing patch baseline \(AWS CLI\)<a name="tagging-patch-baselines-update-command-line"></a>

1. Using your preferred command line tool, run the following command to view the list of patch baselines that you can tag\.

   ```
   aws ssm describe-patch-baselines --filters "Key=OWNER,Values=[Self]"
   ```

   Note the ID of a patch baseline that you want to tag\.

1. Run the following command to tag a patch baseline\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm add-tags-to-resource \
       --resource-type "PatchBaseline" \
       --resource-id "baseline-id" \
       --tags "Key=tag-key,Value=tag-value"
   ```

------
#### [ Windows ]

   ```
   aws ssm add-tags-to-resource ^
       --resource-type "PatchBaseline" ^
       --resource-id "baseline-id" ^
       --tags "Key=tag-key,Value=tag-value"
   ```

------

   If successful, the command has no output\.

1. Run the following command to verify the patch baseline tags\.

------
#### [ Linux & macOS ]

   ```
   aws ssm list-tags-for-resource \
       --resource-type "PatchBaseline" \
       --resource-id "baseline-id"
   ```

------
#### [ Windows ]

   ```
   aws ssm list-tags-for-resource ^
       --resource-type "PatchBaseline" ^
       --resource-id "patchbaseline-id"
   ```

------

### Tag a patch baseline \(AWS Tools for PowerShell\)<a name="tagging-patch-baselines-update-ps"></a>

1. Run the following command to list patch baseline that you can tag\.

   ```
   Get-SSMPatchBaseline
   ```

1. Run the following commands to tag a patch baseline\. Replace each *example resource placeholder* with your own information\.

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
       -ResourceType "PatchBaseline" `
       -ResourceId "baseline-id" `
       -Tag $tag `
       -Force
   ```

1. Run the following command to verify the patch baseline tags\.

   ```
   Get-SSMResourceTag `
       -ResourceType "PatchBaseline" `
       -ResourceId "baseline-id"
   ```

## Removing tags from patch baselines<a name="tagging-patch-baselines-remove"></a>

You can use the Systems Manager console or the command line to remove tags from a patch baseline\.

**Topics**
+ [Removing tags from patch baseline \(console\)](#tagging-patch-baselines-remove-console)
+ [Removing tags from patch baselines \(command line\)](#tagging-patch-baselines-remove-command-line)

### Removing tags from patch baseline \(console\)<a name="tagging-patch-baselines-remove-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose the name of the patch baseline to remove tags from, scroll down to the **Tags table** section, and then choose **Edit tags** tab\.

1. Choose **Remove tag** next to the tag pair you no longer need\.

1. Choose **Save changes**\.

### Removing tags from patch baselines \(command line\)<a name="tagging-patch-baselines-remove-command-line"></a>

1. Using your preferred command line tool, run the following command to list the patch baselines in your account\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-patch-baselines
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-patch-baselines
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMPatchBaseline
   ```

------

   Note the ID of a patch baseline from which you want to remove tags\.

1. Run the following command to remove tags from a patch baseline\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm remove-tags-from-resource \
       --resource-type "PatchBaseline" \
       --resource-id "baseline-id" \
       --tag-key "tag-key"
   ```

------
#### [ Windows ]

   ```
   aws ssm remove-tags-from-resource ^
       --resource-type "PatchBaseline" ^
       --resource-id "baseline-id" ^
       --tag-key "tag-key"
   ```

------
#### [ PowerShell ]

   ```
   Remove-SSMResourceTag `
       -ResourceType "PatchBaseline" `
       -ResourceId "baseline-id" `
       -TagKey "tag-key
   ```

------

   If successful, the command has no output\.

1. Run the following command to verify the patch baseline tags\.

------
#### [ Linux & macOS ]

   ```
   aws ssm list-tags-for-resource \
       --resource-type "PatchBaseline" \
       --resource-id "baseline-id"
   ```

------
#### [ Windows ]

   ```
   aws ssm list-tags-for-resource ^
       --resource-type "PatchBaseline" ^
       --resource-id "baseline-id"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMResourceTag `
       -ResourceType "PatchBaseline" `
       -ResourceId "baseline-id"
   ```

------