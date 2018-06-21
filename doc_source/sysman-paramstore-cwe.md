# Setting Up Notifications and Events for Systems Manager Parameters<a name="sysman-paramstore-cwe"></a>

You can use Amazon CloudWatch Events and Amazon SNS to notify you about changes to Systems Manager Parameters\. You can be notified when a parameter is created, updated, or deleted\. 

You can also use CloudWatch to perform an action on a target for specific parameter events\. This means, for example, that you can execute an AWS Lambda function to recreate a parameter when it is deleted\. You can also set up a notification to trigger a Lambda function when your database password is updated\. The Lambda function can force your database connections to reset or reconnect with the new password\.

**Before You Begin**  
Create an Amazon SNS topic\. For more information, see [Getting Started with Amazon SNS](http://docs.aws.amazon.com/sns/latest/dg/GettingStarted.html) in the *Amazon Simple Notification Service Developer Guide*\.

**To configure CloudWatch Events for Systems Manager Parameters**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Events**, and then choose **Create rule**\.

1. Under **Event Source**, verify that **Event Pattern** is selected\.

1. In the **Service Name** field, choose **EC2 Simple Systems Manager \(SSM\)**

1. In the **Event Type** field, choose **Parameter Store**\.

1. Choose the detail types and statuses for which you want to receive notifications, and then choose **Add targets**\.

1. In the **Targets** list, choose a target type\. For example, choose **Lambda function** or choose **SNS topic**\. For information about the different types of targets, see the corresponding AWS Help documentation\. 

1. Scroll down on the page, and then choose **Configure details**\.

1. Specify the rule details, and then choose **Create rule**\.