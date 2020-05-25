# Tagging Systems Manager parameters<a name="tagging-parameters"></a>

The topics in this section describe how to work with tags on Systems Manager parameters\.

**Topics**
+ [Creating parameters with tags](#tagging-parameters-new)
+ [Adding tags to existing parameters](#tagging-parameters-update)
+ [Removing tags from Systems Manager parameters](#tagging-parameters-remove)

## Creating parameters with tags<a name="tagging-parameters-new"></a>

You can add tags to Systems Manager parameters at the time you create them\.

For information, see the following topics:
+ [Create a Systems Manager parameter \(console\)](param-create-console.md)
+ [Create a Systems Manager parameter \(AWS CLI\)](param-create-cli.md)
+ [Create a Systems Manager parameter \(Tools for Windows PowerShell\)](param-create-ps.md)

## Adding tags to existing parameters<a name="tagging-parameters-update"></a>

You can add tags to custom Systems Manager parameters that you own by using the Systems Manager console or the command line\.

**Topics**
+ [Adding tags to an existing parameter \(console\)](#tagging-parameters-update-console)
+ [Adding tags to an existing parameter \(AWS CLI\)](#tagging-parameters-update-command-line)
+ [Adding tags to an existing parameter \(AWS Tools for PowerShell\)](#tagging-parameters-update-ps)

### Adding tags to an existing parameter \(console\)<a name="tagging-parameters-update-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the name of a parameter you have already created, and then choose the **Tags** tab\.

1. In the first box, enter a key for the tag, such as **Environment**\.

1. In the second box, enter a value for the tag, such as **Test**\.

1. Choose **Save**\.

### Adding tags to an existing parameter \(AWS CLI\)<a name="tagging-parameters-update-command-line"></a>

1. Using your preferred command line tool, run the following command to view the list of parameters that you can tag\.

   ```
   aws ssm describe-parameters
   ```

   Note the name of a parameter that you want to tag\.

1. Run the following command to tag a parameter\.

   ```
   aws ssm add-tags-to-resource --resource-type "Parameter" --resource-id "parameter-name" --tags "Key=tag-key,Value=tag-value"
   ```

   If successful, the command has no output\.

   *parameter\-name* is the name of the SSM parameter you want to tag\.

   *tag\-key* is the name of a custom key you supply\. For example, *Environment* or *Project*\.

   *tag\-value* is the custom content for the value you want to supply for that key\. For example, *Production* or *Q321*\.

1. Run the following command to verify the parameter tags\.

   ```
   aws ssm list-tags-for-resource --resource-type "Parameter" --resource-id "parameter-name"
   ```

### Adding tags to an existing parameter \(AWS Tools for PowerShell\)<a name="tagging-parameters-update-ps"></a>

1. Run the following command to list parameters that you can tag\.

   ```
   Get-SSMParameterList
   ```

1. Run the following commands to tag a parameter\.

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
       -ResourceType "Parameter" `
       -ResourceId "parameter-name" `
       -Tag $tag `
       -Force
   ```

   *parameter\-name* the name of the SSM parameter you want to tag\.

   *tag\-key* is the name of a custom key you supply\. For example, *Environment* or *Project*\.

   *tag\-value* is the custom content for the value you want to supply for that key\. For example, *Production* or *Q321*\.

1. Run the following command to verify the parameter tags\.

   ```
   Get-SSMResourceTag `
       -ResourceType "Parameter" `
       -ResourceId "parameter-name"
   ```

## Removing tags from Systems Manager parameters<a name="tagging-parameters-remove"></a>

You can use the Systems Manager console or the command line to remove tags from Systems Manager parameters\.

**Topics**
+ [Removing tags from Systems Manager parameters \(console\)](#tagging-parameters-remove-console)
+ [Removing tags from Systems Manager parameters \(command line\)](#tagging-parameters-remove-command-line)

### Removing tags from Systems Manager parameters \(console\)<a name="tagging-parameters-remove-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the name of the parameter to remove tags from, and then choose the **Tags** tab\.

1. Choose **Remove** next to the tag pair you no longer need\.

1. Choose **Save**\.

### Removing tags from Systems Manager parameters \(command line\)<a name="tagging-parameters-remove-command-line"></a>

1. Using your preferred command line tool, run the following command to list the parameters in your account\.

------
#### [ Linux ]

   ```
   aws ssm describe-parameters
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-parameters
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMParameterList
   ```

------

   Note the name of a parameter from which you want to remove tags\.

1. Run the following command to remove tags from a parameter\.

------
#### [ Linux ]

   ```
   aws ssm remove-tags-from-resource \
       --resource-type "Parameter" \
       --resource-id "parameter-name" \
       --tagkey "tag-key"
   ```

------
#### [ Windows ]

   ```
   aws ssm remove-tags-from-resource ^
       --resource-type "Parameter" ^
       --resource-id "parameter-name" ^
       --tag-key "tag-key"
   ```

------
#### [ PowerShell ]

   ```
   Remove-SSMResourceTag
       -ResourceId "parameter-name"
       -ResourceType "Parameter"
       -TagKey "tag-key
   ```

------

   *parameter\-name* is the name of the SSM parameter from which you want to remove a tag\.

   *tag\-key* is the name of a key assigned to the parameter\. For example, *Environment* or *Quarter*\.

   If successful, the command has no output\.

1. Run the following command to verify the document tags\.

------
#### [ Linux ]

   ```
   aws ssm list-tags-for-resource \
       --resource-type "Parameter" \
       --resource-id "parameter-name"
   ```

------
#### [ Windows ]

   ```
   aws ssm list-tags-for-resource ^
       --resource-type "Parameter" ^
       --resource-id "parameter-name"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMResourceTag `
       -ResourceType "Parameter" `
       -ResourceId "parameter-name"
   ```

------