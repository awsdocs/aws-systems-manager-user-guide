# Creating Systems Manager Documents<a name="create-ssm-doc"></a>

If the Systems Manager public documents limit the actions you want to perform on your managed instances, you can create your own documents\. When creating a new document, we recommend that you use schema version 2\.2 or later\. 

**Before You Begin**  
Before you create an SSM document, we recommend that you read about the different schemas, features, and syntax available for SSM documents\. For more information, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

**Note**  
If you plan to create an SSM document for State Manager, be aware of the following details:  
You can assign multiple documents to a target by creating different State Manager associations that use different documents\. 
You can use a *shared* document with State Manager, as long as you have permission, but you can't associate a shared document to an instance\. If you want to use or share a document that is associated with one or more targets, you must create a copy of the document and then use or share it\. 
If you create a document with conflicting plugins \(e\.g\., domain join and remove from domain\), the last plugin executed will be the final state\. State Manager does not validate the logical sequence or rationality of the commands or plugins in your document\.
When processing documents, instance associations are applied first, and next tagged group associations are applied\. If an instance is part of multiple tagged groups, then the documents that are part of the tagged group will not be executed in any particular order\. If an instance is directly targeted through multiple documents by its instance ID, there is no particular order of execution\. 
If you change the default version of an State Manager document, any association that uses the document will start using the new default version the next time Systems Manager applies the association to the instance\.
If you create an SSM document for State Manager, you must associate the document with your managed instances after you add it the system\. For more information, see [Create an Association \(Console\)](sysman-state-assoc.md)\.


+ [Copy a Document](#copy-document)
+ [Add a Systems Manager Document \(Console\)](#create-ssm-console)
+ [Create an SSM Document \(AWS CLI\)](#create-ssm-document-cli)
+ [Create an SSM Document \(Tools for Windows PowerShell\)](#create-ssm-document-ps)

## Copy a Document<a name="copy-document"></a>

When you create a document, you specify the contents of the document in JSON or YAML\. The easiest way to get started creating SSM documents is to copy an existing sample from one of the Systems Manager public documents\. The following example shows you how to copy a JSON sample\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To copy a Systems Manager document \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose a document\. 

1. Choose **View details**\.

1. Choose the **Content** tab\.

1. Copy the JSON to a text editor and specify the details for your custom document\.

1. Save the file with a \.`json` file extension\.

**To copy a Systems Manager document \(Amazon EC2 Systems Manager\)**

1. In the Amazon EC2 console, expand **Systems Manager Shared Resources**, and then choose **Documents**\.

1. Choose a document\. 

1. In the lower pane, choose the **Content** tab\.

1. Copy the JSON to a text editor and specify the details for your custom document\.

1. Save the file with a \.`json` file extension\.

After you author the content of the document, you can add it SSM using any one of the following procedures\. 

## Add a Systems Manager Document \(Console\)<a name="create-ssm-console"></a>

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**Add a Systems Manager Document \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Create document**\.

1. Type a descriptive name for the document 

1. In the **Document type** list, choose the type of document you want to create\.

1. Delete the brackets in the **Content** field, and then paste the document you created earlier\.

1. Choose **Create document** to save the document\.

**Add a Systems Manager Document \(Amazon EC2 Systems Manager\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Documents**\.

1. Choose **Create Document**\.

1. Type a descriptive name for the document 

1. In the **Document Type** list, choose the type of document you want to create\.

1. Delete the brackets in the **Content** field, and then paste the document you created earlier\.

1. Choose **Create Document** to save the document\.

## Create an SSM Document \(AWS CLI\)<a name="create-ssm-document-cli"></a>

1. Copy and customize an existing document, as described in [Copy a Document](#copy-document)\.

1. Add the document using the AWS CLI\.

   ```
   aws ssm create-document --content file://path to your file\your file --name "document name" --document-type "Command" 
   ```

   **Windows example**

   ```
   aws ssm create-document --content file://c:\temp\PowershellScript.json --name "PowerShellScript" --document-type "Command" 
   ```

   **Linux example**

   ```
   aws ssm create-document --content file:///home/ec2-user/RunShellScript.json  --name "RunShellScript" --document-type "Command" 
   ```

## Create an SSM Document \(Tools for Windows PowerShell\)<a name="create-ssm-document-ps"></a>

1. Copy and customize an existing document, as described in [Copy a Document](#copy-document)\.

1. Add the document using the AWS Tools for Windows PowerShell\.

   ```
   $json = Get-Content C:\your file | Out-String
   New-SSMDocument -DocumentType Command -Name document name -Content $json
   ```