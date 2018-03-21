# Systems Manager Pre\-Defined Documents<a name="predefined-documents"></a>

To help you get started quickly, Systems Manager provides pre\-defined documents\. To view these documents in the AWS Systems Manager console, in the left navigation, choose **Documents**\. After you choose a document, choose **View details** to view information about the document you selected\.

To view these documents in the Amazon EC2 console, expand **Systems Manager Shared Resources**, and then choose **Documents**\. After you choose a document, use the tabs in the lower pane to view information about the document you selected\.

You can also use the AWS CLI and Tools for Windows PowerShell commands to view a list of documents and get descriptions about those documents\.

To view information about documents using the **AWS CLI**, run the following commands:

```
aws ssm list-documents
```

```
aws ssm describe-document --name "document_name"
```

To view information about documents using the **Tools for Windows PowerShell**, run the following commands:

```
Get-SSMDocumentList
```

```
Get-SSMDocumentDescription -Name "document_name"
```