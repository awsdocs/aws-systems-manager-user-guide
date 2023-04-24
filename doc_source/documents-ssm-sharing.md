# Sharing SSM documents<a name="documents-ssm-sharing"></a>

You can share AWS Systems Manager \(SSM\) documents privately or publicly with accounts in the same AWS Region\. To privately share a document, you modify the document permissions and allow specific individuals to access it according to their AWS account ID\. To publicly share an SSM document, you modify the document permissions and specify `All`\. 

**Warning**  
Use shared SSM documents only from trusted sources\. When using any shared document, carefully review the contents of the document before using it so that you understand how it will change the configuration of your instance\. For more information about shared document best practices, see [Best practices for shared SSM documents](#best-practices-shared)\. 

**Limitations**  
As you begin working with SSM documents, be aware of the following limitations\.
+ Only the owner can share a document\.
+ You must stop sharing a document before you can delete it\. For more information, see [Modify permissions for a shared SSM document](#modify-permissions-shared)\.
+ You can share a document with a maximum of 1000 AWS accounts\. You can request an increase to this limit in the [AWS Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase)\. For **Limit type**, choose *EC2 Systems Manager* and describe your reason for the request\.
+ You can publicly share a maximum of five SSM documents\. You can request an increase to this limit in the [AWS Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase)\. For **Limit type**, choose *EC2 Systems Manager* and describe your reason for the request\.
+ Documents can be shared with other accounts in the same AWS Region only\. Cross\-Region sharing isn't supported\.

For more information about Systems Manager service quotas, see [AWS Systems Manager Service Quotas](https://docs.aws.amazon.com/general/latest/gr/ssm.html#limits_ssm)\.

**Topics**
+ [Best practices for shared SSM documents](#best-practices-shared)
+ [Block public sharing for SSM documents](#block-public-access)
+ [Share an SSM document](#ssm-how-to-share)
+ [Modify permissions for a shared SSM document](#modify-permissions-shared)
+ [Using shared SSM documents](#using-shared-documents)

## Best practices for shared SSM documents<a name="best-practices-shared"></a>

Review the following guidelines before you share or use a shared document\. 

**Remove sensitive information**  
Review your AWS Systems Manager \(SSM\) document carefully and remove any sensitive information\. For example, verify that the document doesn't include your AWS credentials\. If you share a document with specific individuals, those users can view the information in the document\. If you share a document publicly, anyone can view the information in the document\.

**Block public sharing for documents**  
Unless your use case requires public sharing to be turned on, we recommend turning on the block public sharing setting for your Systems Manager documents in the **Preferences** section of the Systems Manager Documents console\.

**Restrict Run Command actions using an IAM trust policy**  
Create a restrictive AWS Identity and Access Management \(IAM\) policy for users who will have access to the document\. The IAM policy determines which SSM documents a user can see in either the Amazon Elastic Compute Cloud \(Amazon EC2\) console or by calling `ListDocuments` using the AWS Command Line Interface \(AWS CLI\) or AWS Tools for Windows PowerShell\. The policy also restricts the actions the user can perform with SSM documents\. You can create a restrictive policy so that a user can only use specific documents\. For more information, see [Customer managed policy examples](security_iam_id-based-policy-examples.md#customer-managed-policies)\.

**Use caution when using shared SSM documents**  
Review the contents of every document that is shared with you, especially public documents, to understand the commands that will be run on your instances\. A document could intentionally or unintentionally have negative repercussions after it's run\. If the document references an external network, review the external source before you use the document\. 

**Send commands using the document hash**  
When you share a document, the system creates a Sha\-256 hash and assigns it to the document\. The system also saves a snapshot of the document content\. When you send a command using a shared document, you can specify the hash in your command to ensure that the following conditions are true:  
+ You're running a command from the correct Systems Manager document
+ The content of the document hasn't changed since it was shared with you\.
If the hash doesn't match the specified document or if the content of the shared document has changed, the command returns an `InvalidDocument` exception\. The hash can't verify document content from external locations\.

## Block public sharing for SSM documents<a name="block-public-access"></a>

Unless your use case requires public sharing to be turned on, we recommend turning on the block public sharing setting for your AWS Systems Manager \(SSM\) documents\. Turning on this setting prevents unwanted access to your SSM documents\. The block public sharing setting is an account level setting that can differ for each AWS Region\. Complete the following tasks to block public sharing for your SSM documents\.

### Block public sharing \(console\)<a name="block-public-access-console"></a>

**To block public sharing of your SSM documents**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Preferences**, and then choose **Edit** in the **Block public sharing** section\.

1. Select the **Block public sharing** check box, and then choose **Save**\. 

### Block public sharing \(command line\)<a name="block-public-access-cli"></a>

Open the AWS Command Line Interface \(AWS CLI\) or AWS Tools for Windows PowerShell on your local computer and run the following command to block public sharing of your SSM documents\.

------
#### [ Linux & macOS ]

```
aws ssm update-service-setting  \
    --setting-id /ssm/documents/console/public-sharing-permission \
    --setting-value Disable \
    --region 'The AWS Region you want to block public sharing in'
```

------
#### [ Windows ]

```
aws ssm update-service-setting ^
    --setting-id /ssm/documents/console/public-sharing-permission ^
    --setting-value Disable ^
    --region "The AWS Region you want to block public sharing in"
```

------
#### [ PowerShell ]

```
Update-SSMServiceSetting `
    -SettingId /ssm/documents/console/public-sharing-permission `
    -SettingValue Disable `
    –Region The AWS Region you want to block public sharing in
```

------

Confirm the setting value was updated using the following command\.

------
#### [ Linux & macOS ]

```
aws ssm get-service-setting   \
    --setting-id /ssm/documents/console/public-sharing-permission \
    --region The AWS Region you blocked public sharing in
```

------
#### [ Windows ]

```
aws ssm get-service-setting  ^
    --setting-id /ssm/documents/console/public-sharing-permission ^
    --region "The AWS Region you blocked public sharing in"
```

------
#### [ PowerShell ]

```
Get-SSMServiceSetting `
    -SettingId /ssm/documents/console/public-sharing-permission `
    -Region The AWS Region you blocked public sharing in
```

------

### Restricting access to block public sharing with IAM<a name="block-public-access-changes-iam"></a>

You can create AWS Identity and Access Management \(IAM\) policies that restrict users from modifying the block public sharing setting\. This prevents users from allowing unwanted access to your SSM documents\. 

The following is an example of an IAM policy that prevents users from updating the block public sharing setting\. To use this example, you must replace the example Amazon Web Services account ID with your own account ID\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "ssm:UpdateServiceSetting",
            "Resource": "arn:aws:ssm:*:987654321098:servicesetting/ssm/documents/console/public-sharing-permission"
        }
    ]
}
```

## Share an SSM document<a name="ssm-how-to-share"></a>

You can share AWS Systems Manager \(SSM\) documents by using the Systems Manager console\. You can also share SSM documents programmatically by calling the `ModifyDocumentPermission` API operation using the AWS Command Line Interface \(AWS CLI\), AWS Tools for Windows PowerShell, or the AWS SDK\. Before you share a document, get the AWS account IDs of the people with whom you want to share\. You will specify these account IDs when you share the document\.

### Share a document \(console\)<a name="share-using-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. In the documents list, choose the document you want to share, and then choose **View details**\. On the **Permissions** tab, verify that you're the document owner\. Only a document owner can share a document\.

1. Choose **Edit**\.

1. To share the command publicly, choose **Public** and then choose **Save**\. To share the command privately, choose **Private**, enter the AWS account ID, choose **Add permission**, and then choose **Save**\. 

### Share a document \(command line\)<a name="share-using-cli"></a>

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

## Modify permissions for a shared SSM document<a name="modify-permissions-shared"></a>

If you share a command, users can view and use that command until you either remove access to the AWS Systems Manager \(SSM\) document or delete the SSM document\. However, you can't delete a document as long as it's shared\. You must stop sharing it first and then delete it\.

### Stop sharing a document \(console\)<a name="unshare-using-console"></a>

**Stop sharing a document**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. In the documents list, choose the document you want to stop sharing, and then choose **View details**\. On the **Permissions** tab, verify that you're the document owner\. Only a document owner can stop sharing a document\.

1. Choose **Edit**\.

1. Choose **X** to delete the AWS account ID that should no longer have access to the command, and then choose **Save**\. 

### Stop sharing a document \(command line\)<a name="unshare-using-cli"></a>

Open the AWS CLI or AWS Tools for Windows PowerShell on your local computer and run the following command to stop sharing a command\.

------
#### [ Linux & macOS ]

```
aws ssm modify-document-permission \
    --name document name \
    --permission-type Share \
    --account-ids-to-remove 'AWS account ID'
```

------
#### [ Windows ]

```
aws ssm modify-document-permission ^
    --name document name ^
    --permission-type Share ^
    --account-ids-to-remove "AWS account ID"
```

------
#### [ PowerShell ]

```
Edit-SSMDocumentPermission `
    -Name document name `
    -PermissionType Share `
    –AccountIdsToRemove AWS account ID
```

------

## Using shared SSM documents<a name="using-shared-documents"></a>

When you share an AWS Systems Manager \(SSM\) document, the system generates an Amazon Resource Name \(ARN\) and assigns it to the command\. If you select and run a shared document from the Systems Manager console, you don't see the ARN\. However, if you want to run a shared SSM document using a method other than the Systems Manager console, you must specify the full ARN of the document for the `DocumentName` request parameter\. You're shown the full ARN for an SSM document when you run the command to list documents\. 

**Note**  
You aren't required to specify ARNs for AWS public documents \(documents that begin with `AWS-*`\) or documents that you own\.

### Use a shared SSM document \(command line\)<a name="using-shared-documents-cli"></a>

 **To list all public SSM documents** 

------
#### [ Linux & macOS ]

```
aws ssm list-documents \
    --filters Key=Owner,Values=Public
```

------
#### [ Windows ]

```
aws ssm list-documents ^
    --filters Key=Owner,Values=Public
```

------
#### [ PowerShell ]

```
$filter = New-Object Amazon.SimpleSystemsManagement.Model.DocumentKeyValuesFilter
$filter.Key = "Owner"
$filter.Values = "Public"

Get-SSMDocumentList `
    -Filters @($filter)
```

------

 **To list private SSM documents that have been shared with you** 

------
#### [ Linux & macOS ]

```
aws ssm list-documents \
    --filters Key=Owner,Values=Private
```

------
#### [ Windows ]

```
aws ssm list-documents ^
    --filters Key=Owner,Values=Private
```

------
#### [ PowerShell ]

```
$filter = New-Object Amazon.SimpleSystemsManagement.Model.DocumentKeyValuesFilter
$filter.Key = "Owner"
$filter.Values = "Private"

Get-SSMDocumentList `
    -Filters @($filter)
```

------

 **To list all SSM documents available to you** 

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

 **To get information about an SSM document that has been shared with you** 

------
#### [ Linux & macOS ]

```
aws ssm describe-document \
    --name arn:aws:ssm:us-east-2:12345678912:document/documentName
```

------
#### [ Windows ]

```
aws ssm describe-document ^
    --name arn:aws:ssm:us-east-2:12345678912:document/documentName
```

------
#### [ PowerShell ]

```
Get-SSMDocumentDescription `
    –Name arn:aws:ssm:us-east-2:12345678912:document/documentName
```

------

 **To run a shared SSM document** 

------
#### [ Linux & macOS ]

```
aws ssm send-command \
    --document-name arn:aws:ssm:us-east-2:12345678912:document/documentName \
    --instance-ids ID
```

------
#### [ Windows ]

```
aws ssm send-command ^
    --document-name arn:aws:ssm:us-east-2:12345678912:document/documentName ^
    --instance-ids ID
```

------
#### [ PowerShell ]

```
Send-SSMCommand `
    –DocumentName arn:aws:ssm:us-east-2:12345678912:document/documentName `
    –InstanceIds ID
```

------