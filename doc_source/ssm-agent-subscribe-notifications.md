# Subscribing to SSM Agent notifications<a name="ssm-agent-subscribe-notifications"></a>

Amazon Simple Notification Service \(Amazon SNS\) can notify you when new versions of AWS Systems Manager Agent \(SSM Agent\) are released\. Use the following procedure to subscribe to these notifications\.

**Tip**  
You can also subscribe to notifications by watching the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub\.

**To subscribe to SSM Agent notifications**

1. Open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

1. From the Region selector in the navigation bar, choose **US East \(N\. Virginia\)**, if it isn't selected already\. You must select this AWS Region because the Amazon SNS notifications for SSM Agent that you're subscribing to are generated from this Region only\.

1. In the navigation pane, choose **Subscriptions**\.

1. Choose **Create subscription**\.

1. For **Create subscription**, do the following:

   1. For **Topic ARN**, use the following Amazon Resource Name \(ARN\):

      `arn:aws:sns:us-east-1:720620558202:SSM-Agent-Update`

   1. For **Protocol**, choose `Email` or `SMS`\.

   1. For **Endpoint**, depending on whether you chose `Email` or `SMS` in the previous step, enter an email address or an area code and number to receive notifications\. 

   1. Choose **Create subscription**\.

1. If you chose `Email`, you will receive an email message asking you to confirm your subscription\. Open the message, and follow the directions to complete your subscription\.

Whenever a new version of SSM Agent is released, we send notifications to subscribers\. If you no longer want to receive these notifications, use the following procedure to unsubscribe\.

**To unsubscribe from SSM Agent notifications**

1. Open the Amazon SNS console\.

1. In the navigation pane, choose **Subscriptions**\.

1. Select the subscription, and then choose **Delete**\. When prompted for confirmation, choose **Delete**\.