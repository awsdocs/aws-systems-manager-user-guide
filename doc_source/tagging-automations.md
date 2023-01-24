# Tagging automations<a name="tagging-automations"></a>

The topics in this section describe how to work with tags on automations\. You can add a maximum of five tags to AWS Systems Manager automations\. You can add tags to automations at the time you initiate them from either the console or the command line, or after they've run by using the command line\.

## Adding tags to automations \(console\)<a name="tagging-automations-add-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Automation**\.

1. Choose the Automation runbook you want to run\.

1. Select **Execute automation**\.

1. In the **Tags** section, choose **Edit**, and then add one or more key\-value tag pairs\.

1. Choose **Save**\.

## Adding tags to automations \(command line\)<a name="tagging-automations-add-cli"></a>

Using your preferred command line tool, run the following command to add tags to an automation when it starts\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

```
aws ssm start-automation-execution \
    --document-name DocumentName \
    --parameters ParametersRequiredByDocument \
    --tags "Key=ExampleKey,Value=ExampleValue"
```

------
#### [ Windows ]

```
aws ssm start-automation-execution ^
    --document-name DocumentName ^
    --parameters ParametersRequiredByDocument ^
    --tags "Key=ExampleKey,Value=ExampleValue"
```

------
#### [ PowerShell ]

```
$exampleTag = New-Object Amazon.SimpleSystemsManagement.Model.Tag
$exampleTag.Key = "ExampleKey"
$exampleTag.Value = "ExampleValue"

Start-SSMAutomationExecution `
    -DocumentName DocumentName `
    -Parameter ParametersRequiredByDocument
    -Tag $exampleTag
```

------

1. You can also tag automations after they run by using your preferred command line tool\. Run the following command to add tags to an automation\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm add-tags-to-resource \
       --resource-type "Automation" \
       --resource-id "automation-execution-id" \
       --tags "Key=ExampleKey,Value=ExampleValue"
   ```

------
#### [ Windows ]

   ```
   aws ssm add-tags-to-resource ^
       --resource-type "Automation" ^
       --resource-id "automation-execution-id" ^
       --tags "Key=ExampleKey,Value=ExampleValue"
   ```

------
#### [ PowerShell ]

   ```
   $exampleTag = New-Object Amazon.SimpleSystemsManagement.Model.Tag 
   $exampleTag.Key = "ExampleKey"
   $exampleTag.Value = "ExampleValue"
   
   Add-SSMResourceTag `
       -ResourceType "Automation" `
       -ResourceId "automation-execution-id" `
       -Tag $exampleTag `
       -Force
   ```

------

   If successful, the command has no output\.

1. Run the following command to verify the automation's tags\.

------
#### [ Linux & macOS ]

   ```
   aws ssm list-tags-for-resource \
       --resource-type "Automation" \
       --resource-id "automation-execution-id"
   ```

------
#### [ Windows ]

   ```
   aws ssm list-tags-for-resource ^
       --resource-type "Automation" ^
       --resource-id "automation-execution-id"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMResourceTag `
       -ResourceType "Automation" `
       -ResourceId "automation-execution-id"
   ```

------

## Removing tags from automations<a name="tagging-automations-remove"></a>

You can use a command line tool to remove tags from an automation\.

### Removing tags from automations \(command line\)<a name="tagging-automations-remove-cli"></a>

1. Using your preferred command line tool, run the following command to remove a tag from an automation\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm remove-tags-from-resource \
       --resource-type "Automation" \
       --resource-id "automation-execution-id" \
       --tag-key "tag-key"
   ```

------
#### [ Windows ]

   ```
   aws ssm remove-tags-from-resource ^
       --resource-type "Automation" ^
       --resource-id "automation-execution-id" ^
       --tag-key "tag-key"
   ```

------
#### [ PowerShell ]

   ```
   Remove-SSMResourceTag `
       -ResourceId "automation-execution-id" `
       -ResourceType "Automation" `
       -TagKey "tag-key" `
       -Force
   ```

------

1. Run the following command to verify the automation's tags\.

------
#### [ Linux & macOS ]

   ```
   aws ssm list-tags-for-resource \
       --resource-type "Automation" \
       --resource-id "automation-execution-id"
   ```

------
#### [ Windows ]

   ```
   aws ssm list-tags-for-resource ^
       --resource-type "Automation" ^
       --resource-id "automation-execution-id"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMResourceTag `
       -ResourceType "Automation" `
       -ResourceId "automation-execution-id"
   ```

------