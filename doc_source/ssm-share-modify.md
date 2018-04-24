# Modify Permissions for a Shared Document<a name="ssm-share-modify"></a>

If you share a command, users can view and use that command until you either remove access to the Systems Manager document or delete the Systems Manager document\. However, you cannot delete a document as long as it is shared\. You must stop sharing it first and then delete it\.

**Topics**
+ [Stop Sharing a Document \(Console\)](#unshare-using-console)
+ [Stop Sharing a Document \(AWS CLI\)](#unshare-using-cli)
+ [Stop Sharing a Document \(AWS Tools for Windows PowerShell\)](#unshare-using-ps)

## Stop Sharing a Document \(Console\)<a name="unshare-using-console"></a>

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**Stop sharing a document \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. In the documents list, choose the document you want to stop sharing, and then choose **View details**\. On the **Permissions** tab, verify that you are the document owner\. Only a document owner can stop sharing a document\.

1. Choose **Edit**\.

1. Choose **X** to delete the AWS account ID that should no longer have access to the command, and then choose **Save**\. 

**Stop sharing a document \(Amazon EC2 Systems Manager\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Documents**\.

1. In the documents list, choose the document you want to stop sharing\. Choose the **Permissions** tab and verify that you are the document owner\. Only a document owner can stop sharing a document\.

1. Choose **Edit**\.

1. Delete the AWS account ID that should no longer have access to the command, and then choose **Save**\. 

## Stop Sharing a Document \(AWS CLI\)<a name="unshare-using-cli"></a>

Open the AWS CLI on your local computer and execute the following command to stop sharing a command\.

```
aws ssm modify-document-permission --name document name --permission-type Share --account-ids-to-remove 'AWS account ID'
```

## Stop Sharing a Document \(AWS Tools for Windows PowerShell\)<a name="unshare-using-ps"></a>

Open **AWS Tools for Windows PowerShell** on your local computer and execute the following command to stop sharing a command\. 

```
Edit-SSMDocumentPermission -Name document name –AccountIdsToRemove AWS account ID -PermissionType Share
```