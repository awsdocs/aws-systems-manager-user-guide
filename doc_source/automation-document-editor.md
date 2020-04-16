# Creating an Automation document using the Editor<a name="automation-document-editor"></a>

If the AWS Systems Manager public Automation documents don't perform all the actions you want to perform on your AWS resources, you can create your own documents\. For example, you can use the editor to modify parameters, add additional steps to an existing Automation document, or combine multiple Automation documents into a single document\. If you're familiar with writing your own Automation documents in JSON or YAML, you can use the editor to enter the JSON or YAML document content\.

For examples of custom Automation documents, see [Sample scenarios and custom Automation document solutions](automation-document-samples.md)\.

**Note**  
If your Automation document uses the `aws:executeScript` Automation action with the `Attachment` input parameter, you must use the AWS CLI or Document Builder to successfully create the document\.

The following procedure describes how to use the editor to create an Automation document\.

**To create an Automation document using the editor**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Create automation**\.

1. For **Name**, type a descriptive name for the document\.

1. Choose the **Editor** tab, and choose **Edit**\.

1. Enter the document content using JSON or YAML\.

1. Choose **Create automation**\.