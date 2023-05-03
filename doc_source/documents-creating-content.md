# Creating SSM document content<a name="documents-creating-content"></a>

If the AWS Systems Manager public documents don't perform all the actions you want to perform on your AWS resources, you can create your own SSM documents\. You can also clone SSM documents using the console\. Cloning documents copies content from an existing document to a new document that you can modify\. When creating or cloning a document, the content of the document must not exceed 64KB\. This quota also includes the content specified for input parameters at runtime\. When you create a new `Command` or `Policy` document, we recommend that you use schema version 2\.2 or later so you can take advantage of the latest features, such as document editing, automatic versioning, sequencing, and more\.

## Writing SSM document content<a name="writing-ssm-doc-content"></a>

To create your own SSM document content, it's important to understand the different schemas, features, plugins, and syntax available for SSM documents\. We recommend becoming familiar with the following resources\.
+  [Writing your own AWS Systems Manager documents](http://aws.amazon.com/blogs/mt/writing-your-own-aws-systems-manager-documents/) 
+  [Data elements and parameters](documents-syntax-data-elements-parameters.md) 
+  [Schemas, features, and examples](documents-schemas-features.md) 
+  [Command document plugin reference](documents-command-ssm-plugin-reference.md) 
+  [Systems Manager Automation actions reference](automation-actions.md) 
+  [Automation system variables](automation-variables.md) 
+  [Additional runbook examples](automation-document-examples.md) 
+  [Working with Systems Manager Automation runbooks](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/systems-manager-automation-docs.html) using the AWS Toolkit for Visual Studio Code 
+  [Using Document Builder to create runbooks](automation-document-builder.md) 
+  [Using scripts in runbooks](automation-document-script-considerations.md) 

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

You can clone AWS Systems Manager documents using the Systems Manager Documents console to create SSM documents\. Cloning SSM documents copies content from an existing document to a new document that you can modify\. You can't clone a document larger than 64KB\.

**To clone an SSM document**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. In the search box, enter the name of the document you want to clone\.

1. Choose the name of the document you want to clone, and then choose **Clone document** in the **Actions** dropdown\. 

1. Modify the document as you prefer, and then choose **Create document** to save the document\. 

After writing your SSM document content, you can use your content to create an SSM document using one of the following methods\.

**Topics**
+ [Writing SSM document content](#writing-ssm-doc-content)
+ [Cloning an SSM document](#cloning-ssm-document)
+ [Creating composite documents](#documents-creating-composite)

## Creating composite documents<a name="documents-creating-composite"></a>

A *composite* AWS Systems Manager \(SSM\) document is a custom document that performs a series of actions by running one or more secondary SSM documents\. Composite documents promote *infrastructure as code* by allowing you to create a standard set of SSM documents for common tasks such as boot\-strapping software or domain\-joining instances\. You can then share these documents across AWS accounts in the same AWS Region to reduce SSM document maintenance and ensure consistency\.

For example, you can create a composite document that performs the following actions:

1. Installs all patches in the allow list\.

1. Installs antivirus software\.

1. Downloads scripts from GitHub and runs them\.

In this example, your custom SSM document includes the following plugins to perform these actions:

1. The `aws:runDocument` plugin to run the `AWS-RunPatchBaseline` document, which installs all allow listed patches\.

1. The `aws:runDocument` plugin to run the `AWS-InstallApplication` document, which installs the antivirus software\.

1. The `aws:downloadContent` plugin to download scripts from GitHub and run them\.

Composite and secondary documents can be stored in Systems Manager, GitHub \(public and private repositories\), or Amazon S3\. Composite documents and secondary documents can be created in JSON or YAML\. 

**Note**  
Composite documents can only run to a maximum depth of three documents\. This means that a composite document can call a child document; and that child document can call one last document\.

To create a composite document, add the [`aws:runDocument`](documents-command-ssm-plugin-reference.md#aws-rundocument) plugin in a custom SSM document and specify the required inputs\. The following is an example of a composite document that performs the following actions:

1. Runs the [`aws:downloadContent`](documents-command-ssm-plugin-reference.md#aws-downloadContent) plugin to download an SSM document from a GitHub public repository to a local directory called bootstrap\. The SSM document is called StateManagerBootstrap\.yml \(a YAML document\)\.

1. Runs the `aws:runDocument` plugin to run the StateManagerBootstrap\.yml document\. No parameters are specified\.

1. Runs the `aws:runDocument` plugin to run the `AWS-ConfigureDocker pre-defined` SSM document\. The specified parameters install Docker on the instance\.

```
{
  "schemaVersion": "2.2",
  "description": "My composite document for bootstrapping software and installing Docker.",
  "parameters": {
  },
  "mainSteps": [
    {
      "action": "aws:downloadContent",
      "name": "downloadContent",
      "inputs": {
        "sourceType": "GitHub",
        "sourceInfo": "{\"owner\":\"TestUser1\",\"repository\":\"TestPublic\", \"path\":\"documents/bootstrap/StateManagerBootstrap.yml\"}",
        "destinationPath": "bootstrap"
      }
    },
    {
      "action": "aws:runDocument",
      "name": "runDocument",
      "inputs": {
        "documentType": "LocalPath",
        "documentPath": "bootstrap",
        "documentParameters": "{}"
      }
    },
    {
      "action": "aws:runDocument",
      "name": "configureDocker",
      "inputs": {
        "documentType": "SSMDocument",
        "documentPath": "AWS-ConfigureDocker",
        "documentParameters": "{\"action\":\"Install\"}"
      }
    }
  ]
}
```

**More info**  
+ For information about rebooting servers and instances when using Run Command to call scripts, see [Handling reboots when running commands](send-commands-reboot.md)\.
+ For more information about the plugins you can add to a custom SSM document, see [Command document plugin reference](documents-command-ssm-plugin-reference.md)\.
+ If you simply want to run a document from a remote location \(without creating a composite document\), see [Running documents from remote locations](documents-running-remote-github-s3.md)\.