# Create an SSM document \(console\)<a name="create-ssm-console"></a>

After you create the content for your custom SSM document, as described in [Writing SSM document content](create-ssm-doc.md#writing-ssm-doc-content), you can use the Systems Manager console to create an SSM document using your content\.

**To create an SSM document \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Create command or session**\.

1. Type a descriptive name for the document 

1. \(Optional\) For **Target type**, specify the type of resources the document can run on\.

1. In the **Document type** list, choose the type of document you want to create\.

1. Delete the brackets in the **Content** field, and then paste the document content you created earlier\.

1. \(Optional\) In the **Document tags** section, apply one or more tag key name/value pairs to the document\.

   Tags are optional metadata that you assign to a resource\. Tags enable you to categorize a resource in different ways, such as by purpose, owner, or environment\. For example, you might want to tag a document to identify the type of tasks it runs, the type of operating systems it targets, and the environment it runs in\. In this case, you could specify the following key name/value pairs:
   + `Key=TaskType,Value=MyConfigurationUpdate`
   + `Key=OS,Value=AMAZON_LINUX_2`
   + `Key=Environment,Value=Production`

   For more information about tagging Systems Manager resources, see [Tagging Systems Manager resources](tagging-resources.md)\.

1. Choose **Create document** to save the document\.