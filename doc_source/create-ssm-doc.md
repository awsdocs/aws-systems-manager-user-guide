# Creating SSM documents<a name="create-ssm-doc"></a>

If the AWS Systems Manager public documents don't perform all the actions you want to perform on your AWS resources, you can create your own SSM documents\. You can also clone SSM documents using the console\. Cloning documents copies content from an existing document to a new document that you can modify\. When you create a new `Command` or `Policy` document, we recommend that you use schema version 2\.2 or later so you can take advantage of the latest features, such as document editing, automatic versioning, sequencing, and more\.

## Writing SSM document content<a name="writing-ssm-doc-content"></a>

To create your own SSM document content, it's important to understand the different schemas, features, plugins, and syntax available for SSM documents\. We recommend becoming familiar with the following resources\.
+  [Writing your own AWS Systems Manager documents](http://aws.amazon.com/blogs/mt/writing-your-own-aws-systems-manager-documents/) 
+  [SSM document syntax](sysman-doc-syntax.md) 
+  [SSM document schema features and examples](document-schemas-features.md) 
+  [Systems Manager Command document plugin reference](ssm-plugins.md) 
+  [Systems Manager Automation actions reference](automation-actions.md) 
+  [Automation system variables](automation-variables.md) 
+  [Sample scenarios and custom runbook solutions](automation-document-samples.md) 
+  [Working with Systems Manager Automation runbooks](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/systems-manager-automation-docs.html) using the AWS Toolkit for Visual Studio Code 
+  [Using Document Builder to create a custom runbook](automation-walk-document-builder.md) 
+  [Creating runbooks that run scripts](automation-document-script.md) 

AWS pre\-defined SSM documents might perform some of the actions you require\. You can call these documents by using the `aws:runDocument`, `aws:runCommand`, or `aws:executeAutomation` plugins within your custom SSM document, depending on the document type\. You can also copy portions of those documents into a custom SSM document, and edit the content to meet your requirements\.

**Tip**  
When creating SSM document content, you might change the content and update your SSM document several times while testing\. The following commands update the SSM document with your latest content, and update the document's default version to the latest version of the document\.  
The Linux and Windows commands use the `jq` command line tool to filter the JSON response data\.

```
latestDocVersion=$(aws ssm update-document \
    --content file://path/to/file/documentContent.json \
    --name "ExampleDocument" \
    --document-format JSON \
    --document-version '$LATEST' \
    | jq -r '.DocumentDescription.LatestVersion')

aws ssm update-document-default-version \
    --name "ExampleDocument" \
    --document-version $latestDocVersion
```

```
latestDocVersion=$(aws ssm update-document ^
    --content file://C:\path\to\file\documentContent.json ^
    --name "ExampleDocument" ^
    --document-format JSON ^
    --document-version "$LATEST" ^
    | jq -r '.DocumentDescription.LatestVersion')

aws ssm update-document-default-version ^
    --name "ExampleDocument" ^
    --document-version $latestDocVersion
```

```
$content = Get-Content -Path "C:\path\to\file\documentContent.json" | Out-String
$latestDocVersion = Update-SSMDocument `
    -Content $content `
    -Name "ExampleDocument" `
    -DocumentFormat "JSON" `
    -DocumentVersion '$LATEST' `
    | Select-Object -ExpandProperty LatestVersion

Update-SSMDocumentDefaultVersion `
    -Name "ExampleDocument" `
    -DocumentVersion $latestDocVersion
```

## Cloning an SSM document<a name="cloning-ssm-document"></a>

You can clone AWS Systems Manager documents using the Systems Manager Documents console to create SSM documents\. Cloning SSM documents copies content from an existing document to a new document that you can modify\.

**To clone an SSM document**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. In the search box, enter the name of the document you want to clone\.

1. Choose the name of the document you want to clone, and then choose **Clone document** in the **Actions** dropdown\. 

1. Modify the document as you prefer, and then choose **Create document** to save the document\. 

## Using SSM documents in State Manager Associations<a name="ssm-docs-assoc"></a>

If you create an SSM document for State Manager, a capability of AWS Systems Manager, you must associate the document with your managed instances after you add the document to the system\. For more information, see [Creating associations](sysman-state-assoc.md)\.

Keep in mind the following details when using SSM documents in State Manager associations\.
+ You can assign multiple documents to a target by creating different State Manager associations that use different documents\. 
+ If you create a document with conflicting plugins \(for example, domain join and remove from domain\), the last plugin run will be the final state\. State Manager doesn't validate the logical sequence or rationality of the commands or plugins in your document\.
+ When processing documents, instance associations are applied first, and next tagged group associations are applied\. If an instance is part of multiple tagged groups, then the documents that are part of the tagged group won't be run in any particular order\. If an instance is directly targeted through multiple documents by its instance ID, there is no particular order of execution\. 
+ If you change the default version of an SSM Policy document for State Manager, any association that uses the document will start using the new default version the next time Systems Manager applies the association to the instance\.
+ If you create an association using an SSM document that was shared with you, and then the owner stops sharing the document with you, your associations no longer have access to that document\. However, if the owner shares the same SSM document with you again later, your associations automatically remap to it\.

After writing your SSM document content, you can use your content to create an SSM document using one of the following methods\.

**Topics**
+ [Writing SSM document content](#writing-ssm-doc-content)
+ [Cloning an SSM document](#cloning-ssm-document)
+ [Using SSM documents in State Manager Associations](#ssm-docs-assoc)
+ [Create an SSM document \(console\)](create-ssm-console.md)
+ [Create an SSM document \(command line\)](create-ssm-document-cli.md)
+ [Create an SSM document \(API\)](create-ssm-document-api.md)
+ [Creating composite documents](composite-docs.md)