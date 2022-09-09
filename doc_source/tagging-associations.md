# Tagging Systems Manager associations<a name="tagging-associations"></a>

The topics in this section describe how to work with tags on State Manager associations\. State Manager is a component of AWS Systems Manager\.

**Topics**
+ [Creating associations with tags](#tagging-associations-new)
+ [Adding tags to an existing association](#tagging-associations-update)
+ [Removing tags from an association](#tagging-associations-remove)

## Creating associations with tags<a name="tagging-associations-new"></a>

You can add tags to a State Manager association when you create it by using the AWS CLI\. Adding tags to an association when you create it by using the Systems Manager console isn't supported\. For information, see [Create an association \(command line\)](sysman-state-assoc.md#create-state-manager-association-commandline)\.

## Adding tags to an existing association<a name="tagging-associations-update"></a>

Use the following procedures add tags to an existing State Manager association by using the command line\.

**Topics**
+ [Adding tags to an existing association \(AWS CLI\)](#tagging-associations-update-cli)
+ [Adding tags to an existing association \(AWS Tools for PowerShell\)](#tagging-associations-update-ps)

### Adding tags to an existing association \(AWS CLI\)<a name="tagging-associations-update-cli"></a>

1. Using the AWS CLI, run the following command to list associations that you can tag\.

   ```
   aws ssm describe-associations
   ```

   Note the name of an association that you want to tag\.

1. Run the following command to tag an association\. Replace each *example resource placeholder* with your own information\.

   ```
   aws ssm add-tags-to-resource \
       --resource-type "Association" \
       --resource-id "association-ID" \
       --tags "Key=tag-key,Value=tag-value"
   ```

   If successful, the command has no output\.

1. Run the following command to verify the association tags\.

   ```
   aws ssm list-tags-for-resource --resource-type "Association" --resource-id "association-ID"
   ```

### Adding tags to an existing association \(AWS Tools for PowerShell\)<a name="tagging-associations-update-ps"></a>

1. Run the following command to list associations that you can tag\.

   ```
   Get-SSMAssociationList
   ```

1. Run the following commands to tag a parameter\. Replace each *example resource placeholder* with your own information\.

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
       -ResourceType "Association" `
       -ResourceId "association-ID" `
       -Tag $tag `
       -Force
   ```

1. Run the following command to verify the association tags\.

   ```
   Get-SSMResourceTag `
       -ResourceType "Association" `
       -ResourceId "association-ID"
   ```

## Removing tags from an association<a name="tagging-associations-remove"></a>

You can use the command line to remove tags from a State Manager association\.

### Removing tags from an association \(command line\)<a name="tagging-associations-remove-command-line"></a>

1. Using your preferred command line tool, run the following command to list the associations in your account\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-associations
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-associations
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAssociationList
   ```

------

   Note the name of the association from which you want to remove tags\.

1. Run the following command to remove tags from an association\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm remove-tags-from-resource \
       --resource-type "Association" \
       --resource-id "association-ID" \
       --tag-key "tag-key"
   ```

------
#### [ Windows ]

   ```
   aws ssm remove-tags-from-resource ^
       --resource-type "Association" ^
       --resource-id "association-ID" ^
       --tag-key "tag-key"
   ```

------
#### [ PowerShell ]

   ```
   Remove-SSMResourceTag
       -ResourceId "association-ID"
       -ResourceType "Association"
       -TagKey "tag-key
   ```

------

   If successful, the command has no output\.

1. Run the following command to verify the association tags\.

------
#### [ Linux & macOS ]

   ```
   aws ssm list-tags-for-resource \
       --resource-type "Association" \
       --resource-id "association-ID"
   ```

------
#### [ Windows ]

   ```
   aws ssm list-tags-for-resource ^
       --resource-type "Association" ^
       --resource-id "association-ID"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMResourceTag `
       -ResourceType "Association" `
       -ResourceId "association-ID"
   ```

------