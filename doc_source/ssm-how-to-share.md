# Share a Systems Manager Document<a name="ssm-how-to-share"></a>

You can share Systems Manager document by using the Amazon EC2 console, the AWS Systems Manager console, or by programmatically calling the `ModifyDocumentPermission` API operation using the AWS CLI, AWS Tools for Windows PowerShell, or the AWS SDK\. Before you share a document, get the AWS account IDs of the people with whom you want to share\. You will specify these account IDs when you share the document\.

**Topics**
+ [Share a Document \(Console\)](#share-using-console)
+ [Share a Document \(AWS CLI\)](#share-using-cli)
+ [Share a Document \(AWS Tools for Windows PowerShell\)](#share-using-ps)

## Share a Document \(Console\)<a name="share-using-console"></a>

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**Share a document \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. In the documents list, choose the document you want to share, and then choose **View details**\. On the **Permissions** tab, verify that you are the document owner\. Only a document owner can share a document\.

1. Choose **Edit**\.

1. To share the command publicly, choose **Public** and then choose **Save**\. To share the command privately, choose **Private**, enter the AWS account ID, choose **Add permission**, and then choose **Save**\. 

**Share a document \(Amazon EC2 Systems Manager\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Documents**\.

1. In the documents list, choose the document you want to share\. Choose the **Permissions** tab and verify that you are the document owner\. Only a document owner can share a document\.

1. Choose **Edit**\.

1. To share the command publicly, choose **Public** and then choose **Save**\. To share the command privately, choose **Private**, enter the AWS account ID, choose **Add Permission**, and then choose **Save**\. 

## Share a Document \(AWS CLI\)<a name="share-using-cli"></a>

The following procedure requires that you specify a region for your CLI session\. Run Command is currently available in the following Systems Manager [regions](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region)\.

1. Open the AWS CLI on your local computer and execute the following command to specify your credentials\. 

   ```
   aws config
   
   AWS Access Key ID: [your key]
   AWS Secret Access Key: [your key]
   Default region name: region
   Default output format [None]:
   ```

   *region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

1. Use the following command to list all of the Systems Manager documents that are available for you\. The list includes documents that you created and documents that were shared with you\. 

   ```
   aws ssm list-documents --document-filter-list key=Owner,value=all
   ```

1. Use the following command to get a specific document\.

   ```
   aws ssm get-document --name document name
   ```

1. Use the following command to get a description of the document\.

   ```
   aws ssm describe-document --name document name
   ```

1. Use the following command to view the permissions for the document\.

   ```
   aws ssm describe-document-permission --name document name --permission-type Share
   ```

1. Use the following command to modify the permissions for the document and share it\. You must be the owner of the document to edit the permissions\. This command privately shares the document with a specific individual, based on that person's AWS account ID\.

   ```
   aws ssm modify-document-permission --name document name --permission-type Share --account-ids-to-add AWS account ID
   ```

   Use the following command to share a document publicly\.

   ```
   aws ssm modify-document-permission --name document name --permission-type Share --account-ids-to-add 'all'
   ```

## Share a Document \(AWS Tools for Windows PowerShell\)<a name="share-using-ps"></a>

The following procedure requires that you specify a region for your PowerShell session\. Run Command is currently available in the following Systems Manager [regions](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region)\.

1. Open **AWS Tools for Windows PowerShell** on your local computer and execute the following command to specify your credentials\. 

   ```
   Set-AWSCredentials –AccessKey your key –SecretKey your key
   ```

1. Use the following command to set the region for your PowerShell session\. The example uses the us\-west\-2 region\. 

   ```
   Set-DefaultAWSRegion -Region us-west-2
   ```

1. Use the following command to list all of the Systems Manager documents available for you\. The list includes documents that you created and documents that were shared with you\. 

   ```
   Get-SSMDocumentList -DocumentFilterList (@{"key"="Owner";"value"="All"})
   ```

1. Use the following command to get a specific document\.

   ```
   Get-SSMDocument –Name document name
   ```

1. Use the following command to get a description of the document\.

   ```
   Get-SSMDocumentDescription –Name document name
   ```

1. Use the following command to view the permissions of the document\. 

   ```
   Get- SSMDocumentPermission –Name document name -PermissionType Share
   ```

1. Use the following command to modify the permissions for the document and share it\. You must be the owner of the document to edit the permissions\. This command privately shares the document with a specific individual, based on that person's AWS account ID\.

   ```
   Edit-SSMDocumentPermission –Name document name -PermissionType Share -AccountIdsToAdd AWS account ID
   ```

   Use the following command to share a document publicly\.

   ```
   Edit-SSMDocumentPermission -Name document name -AccountIdsToAdd ('all') -PermissionType Share
   ```