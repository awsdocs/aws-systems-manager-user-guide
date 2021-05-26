# AWS Config recording<a name="quick-setup-config"></a>

With Quick Setup, you can quickly create a configuration recorder powered by AWS Config\. Use the configuration recorder to detect changes in your resource configurations and capture the changes as configuration items\. If you are unfamiliar with AWS Config, we recommend learning more about the service by reviewing the content in the AWS Config Developer Guide before creating a configuration with Quick Setup\. For more information about AWS Config, see [What is AWS Config?](https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html) in the *AWS Config Developer Guide*\.

By default, the configuration recorder records all supported resources in the AWS Region where AWS Config is running\. You can customize the configuration so that only the resource types you specify are recorded\. For more information, see [Selecting which resources AWS Config records](https://docs.aws.amazon.com/config/latest/developerguide/select-resources.html) in the *AWS Config Developer Guide*\.

You are charged service usage fees when AWS Config starts recording configurations\. For pricing information, see [AWS Config pricing](https://aws.amazon.com/config/pricing/)\.

**Note**  
Deleting the Quick Setup **Config recording** configuration type does not stop the configuration recorder\. Changes continue to be recorded, and service usage fees apply until you stop the configuration recorder\. To learn more about managing the configuration recorder, see [Managing the Configuration Recorder](https://docs.aws.amazon.com/config/latest/developerguide/stop-start-recorder.html) in the *AWS Config Developer Guide*\.

To set up AWS Config recording, perform the following tasks in the AWS Systems Manager Quick Setup console\.

**To set up AWS Config recording with Quick Setup**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Quick Setup**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Quick Setup** in the navigation pane\.

1. Choose **Create**\.

1. Choose **Config recording**, and then choose **Next**\.

1. In the **Configuration options** section, choose the AWS resource types you want to record and whether you want to include global resources\.

1. Choose the Region you want AWS Config to use when recording changes made to global resources\. The value you specify determines where API calls originate from when AWS Config gathers information about global resources in your configuration\. The Region you choose must be a Region you specify later in **Targets**\.

1. Create a new Amazon Simple Storage Service \(Amazon S3\) bucket, or choose an existing bucket you want to send configuration snapshots to\.

1. Choose the notification option you prefer\. AWS Config uses Amazon Simple Notification Service \(Amazon SNS\) to notify you about important AWS Config events related to your resources\. If you choose the **Use existing SNS topics** option, you must provide the Amazon Web Services account ID and name of the existing Amazon SNS topic in that account you want to use\. If you target multiple AWS Regions, the topic names must be identical in each Region\.

1. In the **Schedule** section, choose how frequently you want Quick Setup to remediate changes made to resources that differ from your configuration\. The **Default** option runs once\. If you don't want Quick Setup to remediate changes made to resources that differ from your configuration, choose **Disable remediation** under **Custom**\.

1. In the **Targets** section, choose whether to enable AWS Config recording for your entire organization, some of your organizational units \(OUs\), or the account you're currently logged in to\.

   If you choose **Entire organization**, continue to step 12\.

   If you choose **Custom**, continue to step 11\.

1. In the **Target OUs** section, select the check boxes of the OUs and Regions where you want to use AWS Config recording\.

1. Choose **Create**\.