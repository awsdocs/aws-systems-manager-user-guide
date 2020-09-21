# Subscribing to SSM Agent notifications<a name="ssm-agent-subscribe-notifications"></a>

Amazon Simple Notification Service \(Amazon SNS\) can notify you when new versions of SSM Agent are released\. Use the following procedure to subscribe to these notifications\.

**Tip**  
You can also subscribe to notifications by watching the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

**To subscribe to SSM Agent notifications**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

1. From the Region selector in the navigation bar, choose **US East \(N\. Virginia\)**, if it is not selected already\. You must select this Region because the SNS notifications for SSM Agent that you are subscribing to are generated from this Region only\.

1. In the navigation pane, choose **Subscriptions**\.

1. Choose **Create subscription**\.

1. For **Create subscription**, do the following:

   1. For **Topic ARN**, use the following Amazon Resource Name \(ARN\):

      `arn:aws:sns:us-east-1:720620558202:SSM-Agent-Update`

   1. For **Protocol**, choose `Email` or `SMS`\.

   1. For **Endpoint**, type an email address that you can use to receive the notifications\. If you choose `SMS`, type an area code and number\. 

   1. Choose **Create subscription**\.

1. If you chose `Email`, you'll receive an email asking you to confirm your subscription\. Open the email and follow the directions to complete your subscription\.

Whenever a new version of SSM Agent is released, we send notifications to subscribers\. If you no longer want to receive these notifications, use the following procedure to unsubscribe\.

**To unsubscribe from SSM Agent notifications**

1. Open the Amazon SNS console\.

1. In the navigation pane, choose **Subscriptions**\.

1. Select the subscription and then choose **Actions**, **Delete subscriptions**\. When prompted for confirmation, choose **Delete**\.