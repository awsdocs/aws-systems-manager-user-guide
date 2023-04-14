# Share an SSM document<a name="ssm-how-to-share"></a>

You can share AWS Systems Manager \(SSM\) documents by using the Systems Manager console\. You can also share SSM documents programmatically by calling the `ModifyDocumentPermission` API operation using the AWS Command Line Interface \(AWS CLI\), AWS Tools for Windows PowerShell, or the AWS SDK\. Before you share a document, get the AWS account IDs of the people with whom you want to share\. You will specify these account IDs when you share the document\.

## Share a document \(console\)<a name="share-using-console"></a>

**Share a document**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. In the documents list, choose the document you want to share, and then choose **View details**\. On the **Permissions** tab, verify that you're the document owner\. Only a document owner can share a document\.

1. Choose **Edit**\.

1. To share the command publicly, choose **Public** and then choose **Save**\. To share the command privately, choose **Private**, enter the AWS account ID, choose **Add permission**, and then choose **Save**\. 

## Share a document \(command line\)<a name="share-using-cli"></a>

The following procedure requires that you specify an AWS Region for your command line session\.

1. Open the AWS CLI or AWS Tools for Windows PowerShell on your local computer and run the following command to specify your credentials\. 

   In the following command, replace *region* with your own information\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

------
#### [ Linux & macOS ]

   ```
   aws config
   
   AWS Access Key ID: [your key]
   AWS Secret Access Key: [your key]
   Default region name: region
   Default output format [None]:
   ```

------
#### [ Windows ]

   ```
   aws config
   
   AWS Access Key ID: [your key]
   AWS Secret Access Key: [your key]
   Default region name: region
   Default output format [None]:
   ```

------
#### [ PowerShell ]

   ```
   Set-AWSCredentials –AccessKey your key –SecretKey your key
   Set-DefaultAWSRegion -Region region
   ```

------

1. Use the following command to list all of the SSM documents that are available for you\. The list includes documents that you created and documents that were shared with you\.

------
#### [ Linux & macOS ]

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

1. Use the following command to get a specific document\.

------
#### [ Linux & macOS ]

   ```
   aws ssm get-document \
       --name document name
   ```

------
#### [ Windows ]

   ```
   aws ssm get-document ^
       --name document name
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMDocument `
       –Name document name
   ```

------

1. Use the following command to get a description of the document\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-document \
       --name document name
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-document ^
       --name document name
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMDocumentDescription `
       –Name document name
   ```

------

1. Use the following command to view the permissions for the document\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-document-permission \
       --name document name \
       --permission-type Share
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-document-permission ^
       --name document name ^
       --permission-type Share
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMDocumentPermission `
       –Name document name `
       -PermissionType Share
   ```

------

1. Use the following command to modify the permissions for the document and share it\. You must be the owner of the document to edit the permissions\. Optionally, you can specify a version of the document you want to share using the `--shared-document-version` parameter\. If you don't specify a version, the system shares the `Default` version of the document\. This example command privately shares the document with a specific individual, based on that person's AWS account ID\.

------
#### [ Linux & macOS ]

   ```
   aws ssm modify-document-permission \
       --name document name \
       --permission-type Share \
       --account-ids-to-add AWS account ID
   ```

------
#### [ Windows ]

   ```
   aws ssm modify-document-permission ^
       --name document name ^
       --permission-type Share ^
       --account-ids-to-add AWS account ID
   ```

------
#### [ PowerShell ]

   ```
   Edit-SSMDocumentPermission `
       –Name document name `
       -PermissionType Share `
       -AccountIdsToAdd AWS account ID
   ```

------

1. Use the following command to share a document publicly\.

------
#### [ Linux & macOS ]

   ```
   aws ssm modify-document-permission \
       --name document name \
       --permission-type Share \
       --account-ids-to-add 'all'
   ```

------
#### [ Windows ]

   ```
   aws ssm modify-document-permission ^
       --name document name ^
       --permission-type Share ^
       --account-ids-to-add "all"
   ```

------
#### [ PowerShell ]

   ```
   Edit-SSMDocumentPermission `
       -Name document name `
       -PermissionType Share `
       -AccountIdsToAdd ('all')
   ```

------