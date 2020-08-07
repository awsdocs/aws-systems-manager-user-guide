# Working with parameter labels \(console\)<a name="sysman-paramstore-labels-console"></a>

This section describes how to perform the following tasks by using the AWS Systems Manager console\.
+ [Create a new parameter label \(console\)](#sysman-paramstore-labels-console-create)
+ [View labels attached to a parameter \(console\)](#sysman-paramstore-labels-console-view)
+ [Move a parameter label \(console\)](#sysman-paramstore-labels-console-move)

## Create a new parameter label \(console\)<a name="sysman-paramstore-labels-console-create"></a>

The following procedure describes how to attach a label to a specific version of an *existing* parameter by using the Systems Manager console\. You can't attach a label when you create a parameter\.

**To attach a label to a parameter version by using the console**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the name of a parameter to open the details page for that parameter\.

1. Choose the **History** tab\.

1. Choose the parameter version for which you want to attach a label\.

1. Choose **Attach labels**\.

1. Choose **Add another label**\. 

1. In the text box, enter the label\. To add more labels, choose **Add another label**\. You can attach a maximum of ten labels\.

1. When you are finished attaching labels, choose **Confirm**\.

## View labels attached to a parameter \(console\)<a name="sysman-paramstore-labels-console-view"></a>

A parameter version can have a maximum of ten labels\. The following procedure describes how to view all labels attached to a parameter version by using the Systems Manager console\.

**To view labels attached to a parameter version by using the console**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the name of a parameter to open the details page for that parameter\.

1. Choose the **History** tab\.

1. Locate the parameter version for which you want to view all attached labels\. The **Labels** column shows all labels attached to the parameter version\.

## Move a parameter label \(console\)<a name="sysman-paramstore-labels-console-move"></a>

You can't delete a parameter label after you create it\. You can, however, move a label between versions of a parameter\. The following procedure describes how to move a parameter label to a different version of the same parameter by using the Systems Manager console\.

**To move a label to a different parameter version by using the console**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the name of a parameter to open the details page for that parameter\.

1. Choose the **History** tab\.

1. Choose the parameter version for which you want to move the label\.

1. Choose **Attach labels**\.

1. Choose **Add another label**\. 

1. In the text box, enter the label\. The console notifies you of the label move\.

1. When you are finished, choose **Confirm**\.