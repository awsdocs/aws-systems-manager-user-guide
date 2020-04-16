# Modify permissions for a shared SSM document<a name="ssm-share-modify"></a>

If you share a command, users can view and use that command until you either remove access to the SSM document or delete the SSM document\. However, you cannot delete a document as long as it is shared\. You must stop sharing it first and then delete it\.

**Topics**
+ [Stop sharing a document \(console\)](#unshare-using-console)
+ [Stop sharing a document \(command line\)](#unshare-using-cli)

## Stop sharing a document \(console\)<a name="unshare-using-console"></a>

**Stop sharing a document**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. In the documents list, choose the document you want to stop sharing, and then choose **View details**\. On the **Permissions** tab, verify that you are the document owner\. Only a document owner can stop sharing a document\.

1. Choose **Edit**\.

1. Choose **X** to delete the AWS account ID that should no longer have access to the command, and then choose **Save**\. 

## Stop sharing a document \(command line\)<a name="unshare-using-cli"></a>

Open the AWS CLI or AWS Tools for Windows PowerShell on your local computer and run the following command to stop sharing a command\.

------
#### [ Linux ]

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