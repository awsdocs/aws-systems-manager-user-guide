# Getting started with OpsCenter<a name="OpsCenter-getting-started"></a>

Set up for Systems Manager OpsCenter is integrated with set up for Systems Manager Explorer\. Explorer is a customizable operations dashboard that reports information about your AWS resources\. Explorer displays an aggregated view of operations data \(OpsData\) for your AWS accounts and across Regions\. In Explorer, OpsData includes metadata about your EC2 instances, patch compliance details, and operational work items \(OpsItems\)\. Explorer provides context about how OpsItems are distributed across your business units or applications, how they trend over time, and how they vary by category\. You can group and filter information in Explorer to focus on items that are relevant to you and that require action\. When you identify high priority issues, you can use Systems Manager OpsCenter to run Automation runbooks and quickly resolve those issues\. 

If you already set up OpsCenter, you still need to complete Integrated Setup to verify settings and options\. If you have not set up OpsCenter, then you can use Integrated Setup to get started with both capabilities\. For more information, see [Getting started with Systems Manager Explorer and OpsCenter](Explorer-setup.md)\.

## \(Optional\) Receive OpsItem notifications<a name="OpsCenter-getting-started-sns"></a>

You can configure OpsCenter to send notifications to an Amazon Simple Notification Service \(Amazon SNS\) topic when the systems creates a new OpsItem or updates an existing OpsItem\. Complete the following tasks to receive notifications for OpsItems\.
+ [Task 1: Create and subscribe to an Amazon SNS topic](#OpsCenter-getting-started-sns-create-topic)
+ [Task 2: Update the Amazon SNS access policy](#OpsCenter-getting-started-sns-encryption-policy)
+ [Task 3: Update the AWS KMS access policy \(optional\)](#OpsCenter-getting-started-sns-KMS-policy)
+ [Task 4: Enable default OpsItems rules to send notifications for new OpsItems](#OpsCenter-getting-started-sns-default-rules)

### Task 1: Create and subscribe to an Amazon SNS topic<a name="OpsCenter-getting-started-sns-create-topic"></a>

To receive notifications, you must create and subscribe to an Amazon SNS topic\. For more information, see [Create a Topic](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html) and [Subscribing an Endpoint to an Amazon SNS Topic](https://docs.aws.amazon.com/sns/latest/dg/sns-tutorial-create-subscribe-endpoint-to-topic.html) in the *Amazon Simple Notification Service Developer Guide*\.

**Note**  
To receive notifications, you must specify the Amazon Resource Name \(ARN\) of an Amazon SNS topic that is in the same AWS Region and account as the OpsItem\. If you are using OpsCenter in multiple Regions or accounts, then you must create and subscribe to an Amazon SNS topic in each Region or account where you want to receive OpsItem notifications\. 

### Task 2: Update the Amazon SNS access policy<a name="OpsCenter-getting-started-sns-encryption-policy"></a>

Use the following procedure to update the Amazon SNS access policy so that Systems Manager can publish OpsItem notifications to the Amazon SNS topic you created in task 1\.

1. Sign in to the AWS Management Console and open the Amazon SNS console at [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.

1. In the navigation pane, choose **Topics**\.

1. Choose the topic you created in task 1, and then choose **Edit**\.

1. Expand **Access policy**\.  
![\[Viewing the Access Policy of an Amazon SNS topic\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_SNS_access_policy.png)

1. Add the following Sid block to the existing policy\.

   ```
   {
         "Sid": "Allow OpsCenter to publish to this topic",
         "Effect": "Allow",
         "Principal": {
           "Service": "ssm.amazonaws.com"
         },
         "Action": "SNS:Publish",
          "Resource": "arn:aws:sns:AWS_Region:account_ID:topic_name"
       }
   ```

   Enter this block after the existing Sid block\. In the following example, the new block is entered at line 29\.  
![\[Editing the Access Policy of an Amazon SNS topic\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_SNS_access_policy_2.png)

1. Choose **Save changes**\.

The system now sends notifications to the Amazon SNS topic when OpsItems are created or updated\.

**Important**  
If you configured the Amazon SNS topic with an AWS Key Management Service \(AWS KMS\) server\-side encryption key, then you must complete task 3\. If you want to configure OpsItems created by the default OpsItem rules to publish to the Amazon SNS topic, then you must also complete task 4\.

### Task 3: Update the AWS KMS access policy \(optional\)<a name="OpsCenter-getting-started-sns-KMS-policy"></a>

If you enabled AWS Key Management Service \(AWS KMS\) server\-side encryption for your Amazon SNS topic, then you must also update the access policy of the AWS KMS customer master key you chose when you configured the topic\. Use the following procedure to update the access policy so that Systems Manager can publish OpsItem notifications to the Amazon SNS topic you created in task 1\.

**Note**  
OpsCenter does not support publishing OpsItems to an Amazon SNS topic configured with an AWS managed key\.

1. Open the AWS KMS console at [https://console\.aws\.amazon\.com/kms](https://console.aws.amazon.com/kms)\.

1. To change the AWS Region, use the Region selector in the upper\-right corner of the page\.

1. In the navigation pane, choose **Customer managed keys**\.

1. Choose the ID of the customer master key you chose when you created the topic\.

1. In the **Key policy** section, choose **Switch to policy view**\.

1. Choose **Edit**\.

1. Add the following Sid block to the existing policy\.

   ```
   {
         "Sid": "Allow OpsItems to decrypt the key",
         "Effect": "Allow",
         "Principal": {
           "Service": "ssm.amazonaws.com"
         },
         "Action": ["kms:Decrypt", "kms:GenerateDataKey*"],
          "Resource": "arn:aws:kms:AWS_Region:account_ID:key/key_ID"
       }
   ```

   Enter this block after one of the existing Sid blocks\. In the following example, the new block is entered at line 14\.  
![\[Editing the AWS KMS Access Policy of an Amazon SNS topic\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_SNS_KMS_access_policy.png)

1. Choose **Save changes**\.

### Task 4: Enable default OpsItems rules to send notifications for new OpsItems<a name="OpsCenter-getting-started-sns-default-rules"></a>

Default OpsItems rules in Amazon EventBridge aren't configured with an ARN for Amazon SNS notifications\. Use the following procedure to edit a rule in EventBridge and enter a `notifications` block\. 

**To add a notifications block to a default OpsItem rule**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose the **OpsItems** tab, and then choose **Configure sources**\.

1. Choose the source rule that you want to configure with a `notification` block, as shown in the following example:  
![\[Choosing an EventBridge rule to add an Amazon SNS notifications block\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_SNS_Setup_2.png)

1. On the rule details page, choose **Edit**\.  
![\[Choosing the edit button\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_SNS_Setup_3.png)

1. Scroll to the **Select targets** section\.  
![\[Locating the Select targets section.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_SNS_Setup_4.png)

1. Enter the `notifications` block before the `resources` block, as shown here\.  
![\[Adding a notifications block\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_SNS_Setup.png)

1. Scroll to the bottom of the rule details page and choose **Update**\.

The next time the systems creates an OpsItem for the default rule, it publishes a notification to the Amazon SNS topic\.

## \(Optional\) Create OpsItem guidelines for your organization<a name="OpsCenter-getting-started-guidelines"></a>

We recommend that each organization create a simple set of guidelines that promote consistency when creating and editing OpsItems\. Guidelines make it easier for users to locate and resolve OpsItems\. The guidelines for your organization should define best practices when users enter information into the following OpsItem fields\.

**Note**  
Amazon EventBridge populates the **Title**, **Source**, and **Description** fields of automatically generated OpsItems\. You can edit the **Title** and the **Description** fields, but you can't edit the **Source** field\.


****  

| Field | Description | 
| --- | --- | 
|  **Title**  |  Guidelines should encourage a consistent OpsItem naming experience\. For example, your guidelines might require that each title include information about the impacted resource, the status, the environment, and the name or the alias of the engineer actively working the issue, if applicable\. All OpsItems created by EventBridge include a title that describes the event that caused the creation of the OpsItem, but you can edit these titles\.You can search OpsItems for *Title*:*contains*\. If your naming guidelines encourage consistent use of keywords, you improve your search results\.  | 
|  **Source**  |  Guidelines can include specifying IDs, software version numbers \(if applicable\) or other relevant data to help users identify the origin of the issue\. You can't edit the **Source** field after the OpsItem is created\.  | 
|  **Priority**  |  \(Optional\) Guidelines include determining the highest and lowest priority for your organization, and any service\-level agreements based on priority\. You can specify priority from 1 to 5\.  | 
|  **Description**  |  Guidelines should suggest how much detail about the issue to include and any steps \(if applicable\) for reproducing the issue\.   | 
|  **Notifications**  |  Guidelines should suggest which Amazon Simple Notification Service \(SNS\) topic Amazon Resource Name \(ARN\) to specify when creating or editing OpsItems\. Be aware that SNS notifications are region\-specific\. This means you must specify an ARN that is in the same AWS Region as the OpsItem\.  | 
|  **Related Resources**  |  Guidelines can include details about which Resources should or shouldn't have an ARN specified\. For supported AWS resource types, the ARN creates a deep link to details about the Resource\.   | 
|  **Operational data**  |  You can specify custom data for each OpsItem that provides context about the issue and other relevant data for future reference\. You can specify searchable custom data\. All users with access to the OpsItem Overview page can search for and view this data\. You can also specify private custom data that is only viewable by users who have access to this OpsItem\. Guidelines could specify structure and standards for key\-value pairs\. These key\-value pairs can describe operational data and resolution details, leading to improved search results\.  | 