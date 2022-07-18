# Tagging OpsItems<a name="tagging-opsitems"></a>

The topics in this section describe how to work with tags on OpsItems\.

**Topics**
+ [Creating OpsItems with tags](#tagging-opsitems-new)
+ [Adding tags to existing OpsItems](#tagging-opsitems-update)
+ [Removing tags from Systems Manager OpsItems](#tagging-opsitems-remove)

## Creating OpsItems with tags<a name="tagging-opsitems-new"></a>

You can add tags to custom AWS Systems Manager OpsItems at the time you create them if you use a command line tool\.

For information, see the following topic:
+ [Creating OpsItems by using the AWS CLI](OpsCenter-manually-create-OpsItems.md#OpsCenter-manually-create-OpsItems-CLI)

## Adding tags to existing OpsItems<a name="tagging-opsitems-update"></a>

You can add tags to OpsItems by using a command line tool\.

**Topics**
+ [Adding tags to an existing OpsItem \(command line\)](#tagging-opsitems-update-command-line)

### Adding tags to an existing OpsItem \(command line\)<a name="tagging-opsitems-update-command-line"></a>

**To add tags to an existing OpsItem \(command line\)**

1. Using your preferred command line tool, run the following command to view the list of OpsItem that you can tag\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-ops-items
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-ops-items
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMOpsItemSummary
   ```

------

   Note the ID of an OpsItem that you want to tag\.

1. Run the following command to tag an OpsItem\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm add-tags-to-resource \
       --resource-type "OpsItem" \
       --resource-id "ops-item-id" \
       --tags "Key=tag-key,Value=tag-value"
   ```

------
#### [ Windows ]

   ```
   aws ssm add-tags-to-resource ^
       --resource-type "OpsItem" ^
       --resource-id "ops-item-id" ^
       --tags "Key=tag-key,Value=tag-value"
   ```

------
#### [ PowerShell ]

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
       -ResourceType "OpsItem" `
       -ResourceId "ops-item-id" `
       -Tag $tag `
       -Force
   ```

------

   If successful, the command has no output\.

1. Run the following command to verify the OpsItem tags\.

------
#### [ Linux & macOS ]

   ```
   aws ssm list-tags-for-resource \
       --resource-type "OpsItem" \
       --resource-id "ops-item-id"
   ```

------
#### [ Windows ]

   ```
   aws ssm list-tags-for-resource ^
       --resource-type "OpsItem" ^
       --resource-id "ops-item-id"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMResourceTag `
       -ResourceType "OpsItem" `
       -ResourceId "ops-item-id"
   ```

------

## Removing tags from Systems Manager OpsItems<a name="tagging-opsitems-remove"></a>

You can use a command line tool to remove tags from Systems Manager OpsItems\.

**Topics**
+ [Removing tags from OpsItems \(command line\)](#tagging-opsitems-remove-command-line)

### Removing tags from OpsItems \(command line\)<a name="tagging-opsitems-remove-command-line"></a>

1. Using your preferred command line tool, run the following command to list the OpsItems in your account\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-ops-items
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-ops-items
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMOpsItemSummary
   ```

------

   Note the name of an OpsItem from which you want to remove tags\.

1. Run the following command to remove tags from an OpsItem\.Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm remove-tags-from-resource \
       --resource-type "OpsItem" \
       --resource-id "ops-item-id" \
       --tag-key "tag-key"
   ```

------
#### [ Windows ]

   ```
   aws ssm remove-tags-from-resource ^
       --resource-type "OpsItem" ^
       --resource-id "ops-item-id" ^
       --tag-key "tag-key"
   ```

------
#### [ PowerShell ]

   ```
   Remove-SSMResourceTag `
       -ResourceId "ops-item-id" `
       -ResourceType "OpsItem" `
       -TagKey "tag-key" `
       -Force
   ```

------

   If successful, the command has no output\.

1. Run the following command to verify the OpsItem tags\.

------
#### [ Linux & macOS ]

   ```
   aws ssm list-tags-for-resource \
       --resource-type "OpsItem" \
       --resource-id "ops-item-id"
   ```

------
#### [ Windows ]

   ```
   aws ssm list-tags-for-resource ^
       --resource-type "OpsItem" ^
       --resource-id "ops-item-id"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMResourceTag `
       -ResourceType "OpsItem" `
       -ResourceId "ops-item-id"
   ```

------