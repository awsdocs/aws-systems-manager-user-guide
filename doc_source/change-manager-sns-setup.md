# Configuring Amazon SNS topics for Change Manager notifications<a name="change-manager-sns-setup"></a>

You can configure Change Manager, a capability of AWS Systems Manager, to send notifications to an Amazon Simple Notification Service \(Amazon SNS\) topic for events related to change requests and change templates\. Complete the following tasks to receive notifications for the Change Manager events you add a topic to\.

**Topics**
+ [Task 1: Create and subscribe to an Amazon SNS topic](#change-manager-sns-setup-create-topic)
+ [Task 2: Update the Amazon SNS access policy](#change-manager-sns-setup-encryption-policy)
+ [Task 3: \(Optional\) Update the AWS Key Management Service access policy](#change-manager-sns-setup-KMS-policy)

## Task 1: Create and subscribe to an Amazon SNS topic<a name="change-manager-sns-setup-create-topic"></a>

First, you must create and subscribe to an Amazon SNS topic\. For more information, see [Create a Topic](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html) and [Subscribing an Endpoint to an Amazon SNS Topic](https://docs.aws.amazon.com/sns/latest/dg/sns-tutorial-create-subscribe-endpoint-to-topic.html) in the *Amazon Simple Notification Service Developer Guide*\.

**Note**  
To receive notifications, you must specify the Amazon Resource Name \(ARN\) of an Amazon SNS topic that is in the same AWS Region and AWS account as the delegated administrator account\. 

## Task 2: Update the Amazon SNS access policy<a name="change-manager-sns-setup-encryption-policy"></a>

Use the following procedure to update the Amazon SNS access policy so that Systems Manager can publish Change Manager notifications to the Amazon SNS topic you created in Task 1\. Without completing this task, Change Manager does not have permission to send notifications for the events you add the topic for\.

1. Sign in to the AWS Management Console and open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

1. In the navigation pane, choose **Topics**\.

1. Choose the topic you created in Task 1, and then choose **Edit**\.

1. Expand **Access policy**\.

1. Add and update the following `Sid` block to the existing policy\.

   ```
   {
         "Sid": "Allow Change Manager to publish to this topic",
         "Effect": "Allow",
         "Principal": {
           "Service": "ssm.amazonaws.com"
         },
         "Action": "SNS:Publish",
         "Resource": "arn:aws:sns:region:account-id:topic_name"
   }
   ```

   Enter this block after the existing `Sid` block, and replace *region*, *account\-id*, and *topic\_name* with the appropriate values for the topic you created\.

1. Choose **Save changes**\.

The system now sends notifications to the Amazon SNS topic when the event type you add to topic for occurs\.

**Important**  
If you configured the Amazon SNS topic with an AWS Key Management Service \(AWS KMS\) server\-side encryption key, then you must complete Task 3\.

## Task 3: \(Optional\) Update the AWS Key Management Service access policy<a name="change-manager-sns-setup-KMS-policy"></a>

If you enabled AWS Key Management Service \(AWS KMS\) server\-side encryption for your Amazon SNS topic, then you must also update the access policy of the AWS KMS key you chose when you configured the topic\. Use the following procedure to update the access policy so that Systems Manager can publish Change Manager approval notifications to the Amazon SNS topic you created in Task 1\.

1. Open the AWS KMS console at [https://console\.aws\.amazon\.com/kms](https://console.aws.amazon.com/kms)\.

1. To change the AWS Region, use the Region selector in the upper\-right corner of the page\.

1. In the navigation pane, choose **Customer managed keys**\.

1. Choose the ID of the customer managed key you chose when you created the topic\.

1. In the **Key policy** section, choose **Switch to policy view**\.

1. Choose **Edit**\.

1. Add the following `Sid` block to the existing policy\.

   ```
   {
         "Sid": "Allow Change Manager to decrypt the key",
         "Effect": "Allow",
         "Principal": {
           "Service": "ssm.amazonaws.com"
         },
         "Action": ["kms:Decrypt", "kms:GenerateDataKey*"],
          "Resource": "arn:aws:kms:region:account-id:key/key_ID"
       }
   ```

   Enter this block after one of the existing `Sid` blocks\. 

1. Choose **Save changes**\.