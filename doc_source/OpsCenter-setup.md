# Set up OpsCenter<a name="OpsCenter-setup"></a>

AWS Systems Manager uses an integrated setup experience to help you get started with OpsCenter and Explorer, which are capabilities of Systems Manager\. Explorer is a customizable operations dashboard that reports information about your AWS resources\. In this documentation, Explorer and OpsCenter setup is called *Integrated Setup*\.

You must use Integrated Setup to set up OpsCenter with Explorer\. If you already set up OpsCenter, you still need to complete Integrated Setup to verify settings and options\. Integrated Setup is only available in the AWS Systems Manager console\. You can't set up Explorer and OpsCenter programmatically\. For more information, see [Getting started with Systems Manager Explorer and OpsCenter](Explorer-setup.md)\. 

**To set up OpsCenter**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose **Enable OpsCenter**\.

1. Choose **Enable this option to have Explorer configure AWS Config and Amazon CloudWatch events to automatically create OpsItems**\. If you don't choose this option, OpsCenter remains disabled\.
**Note**  
Amazon EventBridge \(formerly Amazon CloudWatch Events\) provides all functionality of CloudWatch Events and some new features, such as custom event buses, third\-party event sources and schema registry\.

1. Choose **Enable OpsCenter**\.

After you enable OpsCenter, you can do the following:
+ Create CloudWatch alarms using the **Open CloudWatch console** button\. For more information, see [Configure CloudWatch alarms to create OpsItems](OpsCenter-create-OpsItems-from-CloudWatch-Alarms.md)\.
+ Enable operational insights\. For more information, see [Analyzing operational insights to reduce duplicate OpsItems](OpsCenter-working-operational-insights.md)\.
+ Enable AWS Security Hub findings alarms\. For more information, see [Security Hub](OpsCenter-integrate-with-security-hub.md)\.
+ Edit the category, severity, and state of EventBridge rules\. You can choose a rule to view, edit, and manage in EventBridge\. For more information, see [Configure EventBridge rules to create OpsItems](OpsCenter-automatically-create-OpsItems-2.md)\.

After you set up OpsCenter, you can decide if you want to use a central account to create and manage OpsItems for member accounts\. For more information, see [\(Optional\) Setting up OpsCenter to centrally manage OpsItems across accounts](OpsCenter-getting-started-multiple-accounts.md)\. 