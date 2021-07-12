# Viewing SSM Command document content<a name="viewing-ssm-document-content"></a>

To preview the required and optional parameters for an AWS Systems Manager \(SSM\) Command document, in addition to the actions the document runs, you can view the content of the document in the Systems Manager console\.

**To view SSM Command document content**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. In the search box, select **Document type**, and then select **Command**\.

1. Choose the name of a document, and then choose the **Content** tab\. 

1. In the content field, review the available parameters and action steps for the document\.

   For example, the following image shows that \(1\) `version` and \(2\) `allowDowngrade` are optional parameters for the `AWS-UpdateSSMAgent` document, and that the first action run by the document is \(3\) `aws:updateSsmAgent`\.  
![\[View SSM document content in the Systems Manager console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/view-document-content.png)