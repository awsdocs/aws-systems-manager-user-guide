# Copy a Document<a name="copy-document"></a>

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

After you author the content of the document, you can add it to Systems Manager using any one of the following procedures\. 
+ [Add a Systems Manager Document \(Console\)](create-ssm-console.md)
+ [Create an SSM Document \(AWS CLI\)](create-ssm-document-cli.md)
+ [Create an SSM Document \(Tools for Windows PowerShell\)](create-ssm-document-ps.md)