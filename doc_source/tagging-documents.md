# Tagging Systems Manager documents<a name="tagging-documents"></a>

The topics in this section describe how to work with tags on Systems Manager documents\.

**Topics**
+ [Creating documents with tags](#tagging-documents-new)
+ [Adding tags to existing documents](#tagging-documents-update)
+ [Removing tags from Systems Manager documents](#tagging-documents-remove)

## Creating documents with tags<a name="tagging-documents-new"></a>

You can add tags to custom Systems Manager documents at the time you create them\.

For information, see the following topics:
+ [Create an SSM document \(console\)](create-ssm-console.md)
+ [Create an SSM document \(command line\)](create-ssm-document-cli.md)

## Adding tags to existing documents<a name="tagging-documents-update"></a>

You can add tags to custom Systems Manager documents that you own by using the Systems Manager console or the command line\.

**Topics**
+ [Adding tags to an existing SSM document \(console\)](#tagging-documents-update-console)
+ [Adding tags to an existing SSM document \(command line\)](#tagging-documents-update-command-line)

### Adding tags to an existing SSM document \(console\)<a name="tagging-documents-update-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose the **Owned by me** tab\.

1. Choose the name of the document to add tags to, and then choose the **Details** tab\.

1. In the **Tags** section, choose **Edit**, and then add one or more key\-value tag pairs\.

1. Choose **Save**\.

### Adding tags to an existing SSM document \(command line\)<a name="tagging-documents-update-command-line"></a>

**To add tags to an existing document \(command line\)**

1. Using your preferred command line tool, run the following command to view the list of documents that you can tag\.

------
#### [ Linux ]

   ```
   aws ssm list-documents
   ```

------
#### [ Windows ]

   ```
   aws ssm list-documents
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMDocumentList
   ```

------

   Note the name of a document that you want to tag\.

1. Run the following command to tag a document\.

------
#### [ Linux ]

   ```
   aws ssm add-tags-to-resource \
       --resource-type "Document" \
       --resource-id "document-name" \
       --tags "Key=tag-key,Value=tag-value"
   ```

------
#### [ Windows ]

   ```
   aws ssm add-tags-to-resource ^
       --resource-type "Document" ^
       --resource-id "document-name" ^
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
       -ResourceType "Document" `
       -ResourceId "document-name" `
       -Tag $tag `
       -Force
   ```

------

   *tag\-key* is the name of a custom key you supply\. For example, *Region* or *Quarter*\.

   *tag\-value* is the custom content for the value you want to supply for that key\. For example, *West* or *Q321*\.

   *document\-name* is the name of the SSM document you want to tag\.

   If successful, the command has no output\.

1. Run the following command to verify the document tags\.

------
#### [ Linux ]

   ```
   aws ssm list-tags-for-resource \
       --resource-type "Document" \
       --resource-id "document-name"
   ```

------
#### [ Windows ]

   ```
   aws ssm list-tags-for-resource ^
       --resource-type "Document" ^
       --resource-id "document-name"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMResourceTag `
       -ResourceType "Document" `
       -ResourceId "document-name"
   ```

------

## Removing tags from Systems Manager documents<a name="tagging-documents-remove"></a>

You can use the Systems Manager console or the command line to remove tags from Systems Manager documents\.

**Topics**
+ [Removing tags from Systems Manager documents \(console\)](#tagging-documents-remove-console)
+ [Removing tags from Systems Manager documents \(command line\)](#tagging-documents-remove-command-line)

### Removing tags from Systems Manager documents \(console\)<a name="tagging-documents-remove-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose the **Owned by me** tab\.

1. Choose the name of the document to remove tags from, and then choose the **Details** tab\.

1. In the **Tags** section, choose **Edit**, and then choose **Remove** next to the tag pair you no longer need\.

1. Choose **Save**\.

### Removing tags from Systems Manager documents \(command line\)<a name="tagging-documents-remove-command-line"></a>

1. Using your preferred command line tool, run the following command to list the documents in your account\.

------
#### [ Linux ]

   ```
   aws ssm list-documents
   ```

------
#### [ Windows ]

   ```
   aws ssm list-documents
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMDocumentList
   ```

------

   Note the name of a document from which you want to remove tags\.

1. Run the following command to remove tags from a document\.

------
#### [ Linux ]

   ```
   aws ssm remove-tags-from-resource \
       --resource-type "Document" \
       --resource-id "document-name" \
       --tag-key "tag-key"
   ```

------
#### [ Windows ]

   ```
   aws ssm remove-tags-from-resource ^
       --resource-type "Document" ^
       --resource-id "document-name" ^
       --tag-key "tag-key"
   ```

------
#### [ PowerShell ]

   ```
   Remove-SSMResourceTag `
       -ResourceId "document-name" `
       -ResourceType "Document" `
       -TagKey "tag-key" `
       -Force
   ```

------

   *document\-name* is the name of the SSM document from which you want to remove tags\.

   *tag\-key* is the name of a key assigned to the document\. For example, *Environment* or *Quarter*\.

   If successful, the command has no output\.

1. Run the following command to verify the document tags\.

------
#### [ Linux ]

   ```
   aws ssm list-tags-for-resource \
       --resource-type "Document" \
       --resource-id "document-name"
   ```

------
#### [ Windows ]

   ```
   aws ssm list-tags-for-resource ^
       --resource-type "Document" ^
       --resource-id "document-name"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMResourceTag `
       -ResourceType "Document" `
       -ResourceId "document-name"
   ```

------