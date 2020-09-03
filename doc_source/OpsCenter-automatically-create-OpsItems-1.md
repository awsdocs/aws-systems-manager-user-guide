# Configuring CloudWatch to automatically create OpsItems for specific alarms<a name="OpsCenter-automatically-create-OpsItems-1"></a>

You can configure Amazon CloudWatch to automatically create OpsItems in OpsCenter when an alarm reaches the alarm state\.

The `AWSServiceRoleForCloudWatchEvents` service\-linked role enables AWS to perform alarm actions on your behalf\. The first time you create an alarm in the AWS Management Console, the IAM CLI, or the IAM API, CloudWatch creates the service\-linked role for you\. 

Use the following procedure to configure **SSM OpsItems** as the target of a CloudWatch alarm\. When the alarm is trigged, CloudWatch creates a new OpsItem in OpsCenter\.

**To configure OpsCenter as a target of a CloudWatch alarm**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Alarms**, and then either choose to create a new alarm or edit an existing alarm\.

1. After verifying the details of the alarm, choose **Add target**\.

1. In the **Select target type** list, choose **SSM OpsItem**\. 

1. For **Configure input**, verify that **Matched event** is selected\.

1. In the permissions section, choose **Create a new role for this specific resource** to create a new role with the required permissions\. Or, choose **Use existing role** and choose the IAM role you created that gives CloudWatch permission to create OpsItems in OpsCenter\. For more information about the required role and permissions, see [Getting started with OpsCenter](OpsCenter-getting-started.md)\.

After an OpsItem is created from an alarm, you can view the event details by opening the OpsItem and scrolling down to the **Private operational data** section\.

For information about how to configure the options in an OpsItem, see [Working with OpsItems](OpsCenter-working-with-OpsItems.md)\.