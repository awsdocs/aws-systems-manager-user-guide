# Tagging Systems Manager Documents<a name="sysman-ssm-docs-tagging"></a>

You can use the Systems Manager console, the AWS CLI, the AWS Tools for Windows, or the [AddTagsToResource](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_AddTagsToResource.html) API to add tags to Systems Manager resources, including documents, managed instances, Maintenance Windows, Parameter Store parameters, and patch baselines\. 

Tagging is useful when you have many resources of the same type — you can quickly identify a specific resource based on the tags you've assigned to it\. Each tag consists of a *key* and an optional *value*, both of which you define\. 

For example, you can tag documents for specific environments, departments, users, groups, or periods\. After you tag a document, you can restrict access to it by creating an IAM policy that specifies the tags that a user can access\. For more information about restricting access to documents by using tags, see [Controlling Access to Documents Using Tags](#sysman-ssm-docs-tagging-access)\.

**Topics**
+ [Tag a Document \(AWS CLI\)](#sysman-ssm-docs-tagging-cli)
+ [Tag a Document \(AWS Tools for Windows\)](#sysman-ssm-docs-tagging-tfw)
+ [Tag a Document \(Console\)](#sysman-ssm-docs-tagging-console)
+ [Controlling Access to Documents Using Tags](#sysman-ssm-docs-tagging-access)

## Tag a Document \(AWS CLI\)<a name="sysman-ssm-docs-tagging-cli"></a>

1. At a terminal \(Linux, macOS, or Unix\) or command prompt \(Windows\), run the list\-documents command to list the documents that you can tag\.

   ```
   aws ssm list-documents
   ```

   Note the name of a document that you want to tag\.

1. Run the following command to tag a document\.

   ```
   aws ssm add-tags-to-resource --resource-type "Document" --resource-id "document-name" --tags "Key=key,Value=value"
   ```

   *document\-name* the name of the Systems Manager document you want to tag\.

   *key* is the name of a custom key you supply\. For example, *region* or *quarter*\.

   *value* is the custom content for the value you want to supply for that key\. For example, *west* or *Q318*\.

   If successful, the command has no output\.

1. Execute the following command to verify the document tags\.

   ```
   aws ssm list-tags-for-resource --resource-type "Document" --resource-id "document-name"
   ```

## Tag a Document \(AWS Tools for Windows\)<a name="sysman-ssm-docs-tagging-tfw"></a>

1. Open AWS Tools for Windows PowerShell and run the following command to list documents that you can tag:

   ```
   Get-SSMDocumentList
   ```

1. Run the following commands one at a time to tag a document:

   ```
   $tag1 = New-Object Amazon.SimpleSystemsManagement.Model.Tag
   $tag1.Key = "key"
   $tag1.Value = "value"
   Add-SSMResourceTag -ResourceType "Document" -ResourceId "document-name" -Tag $tag1
   ```

   *document\-name* the name of the Systems Manager document you want to tag\.

   *key* is the name of a custom key you supply\. For example, *region* or *quarter*\.

   *value* is the custom content for the value you want to supply for that key\. For example, *west* or *Q318*\.

   If successful, the command has no output\.

1. Run the following command to verify the document tags:

   ```
   Get-SSMResourceTag -ResourceType "Document" -ResourceId "document-name"
   ```

## Tag a Document \(Console\)<a name="sysman-ssm-docs-tagging-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the left navigation, choose **Documents**\.

1. Choose the name of an existing document, and then choose the **Tags** tab\.

1. In the first box, enter a key for the tag, such as **Region**\.

1. In the second box, enter a value for the tag, such as **West**\.

1. Choose **Save**\.

## Controlling Access to Documents Using Tags<a name="sysman-ssm-docs-tagging-access"></a>

After you tag a document, you can restrict access to it by creating an IAM policy that specifies the tags the user can access\. When a user attempts to use a document, the system checks the IAM policy and the tags specified for the document\. If the user does not have access to the tags assigned to the document, the user receives an access denied error\. Use the following procedure to create an IAM policy that restricts access to documents by using tags\.

**Before You Begin**  
Create and tag documents\. For more information, see [Tagging Systems Manager Documents](#sysman-ssm-docs-tagging)\.

**To restrict a user's access to documents by using tags**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. Choose the **JSON** tab\.

1. Copy the following sample policy and paste it into the text field, replacing the sample text\. Replace *tag\_key* and *tag\_value* with the key\-value pair for your tag\.

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Effect":"Allow",
            "Action":[
               "ssm:GetDocument"
            ],
            "Resource":"*",
            "Condition":{
               "StringLike":{
                  "ssm:resourceTag/tag_key":[
                     "tag_value"
                  ]
               }
            }
         }
      ]
   }
   ```

   You can specify multiple keys in the policy by using the following **Condition** format\. Specifying multiple keys creates an *AND* relationship for the keys\.

   ```
   "Condition":{
      "StringLike":{
         "ssm:resourceTag/tag_key1":[
                     "tag_value1"
         ],
         "ssm:resourceTag/tag_key2":[
                     "tag_value2"
         ]
      }
   }
   ```

   You can specify multiple values in the policy by using the following **Condition** format\. **ForAnyValue** establishes an *OR* relationship for the values\. You can also specify **ForAllValues** to establish an AND relationship\.

   ```
   "Condition":{
      "ForAnyValue:StringLike":{
         "ssm:resourceTag/tag_key1":[
            "tag_value1",
            "tag_value2"
         ]
      }
   }
   ```

1. Choose **Review policy**\.

1. In the **Name** field, specify a name that identifies this as a user policy for tagged documents\.

1. Enter a description\.

1. Verify details of the policy in the **Summary** section\.

1. Choose **Create policy**\.

1. Assign the policy to IAM users or groups\. For more information, see [Changing Permissions for an IAM User](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) and [Attaching a Policy to an IAM Group](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_attach-policy.html)\.

After you attach the policy to the IAM user or group account, if a user tries to use a document and the user's policy does not allow the user to access a tag for the document \(call the GetDocument API\), the system returns an error\. The error is similar to the following:

"User: *user\_name* is not authorized to perform: ssm:GetDocument on resource: *document\-name* with the following command\."

If a document has multiple tags, the user will still receive the Access Denied error if the user does not have permission to access any one of those tags\.