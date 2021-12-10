# Deleting custom SSM documents<a name="ssm-deleting"></a>

If you no longer want to use a custom SSM document, you can delete it by using either the AWS Command Line Interface \(AWS CLI\) or the AWS Systems Manager console\. 

**To delete an SSM document \(AWS CLI\)**

1. Before you delete the document, we recommend that you disassociate all instances that are associated with the document\.

   Run the following command to disassociate an instance from a document\.

   ```
   aws ssm delete-association --instance-id "123456789012" --name "documentName"
   ```

   There is no output if the command succeeds\.

1. Run the following command\.

------
#### [ Linux ]

   ```
   aws ssm delete-document \
       --name "document-name" \
       --document-version "document-version" \
       --version-name "version-name"
   ```

------
#### [ Windows ]

   ```
   aws ssm delete-document ^
       --name "document-name" ^
       --document-version "document-version" ^
       --version-name "version-name"
   ```

------
#### [ PowerShell ]

   ```
   Delete-SSMDocument `
       -Name "document-name" `
       -DocumentVersion 'document-version' `
       -VersionName 'version-name'
   ```

------

   There is no output if the command succeeds\.
**Important**  
If the `document-version` or the `version-name` are not provided, all versions of the document are deleted\.

**To delete an SSM document \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Select the document you want to delete\.

1. Select **Delete**\. When prompted to delete the document, select **Delete**\.