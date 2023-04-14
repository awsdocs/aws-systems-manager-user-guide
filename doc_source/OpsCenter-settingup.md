# Setting up OpsCenter<a name="OpsCenter-settingup"></a>

AWS Systems Manager uses an integrated setup experience to help you get started with OpsCenter and Explorer, which are capabilities of Systems Manager\. Explorer is a customizable operations dashboard that reports information about your AWS resources\. In this documentation, Explorer and OpsCenter setup is called *Integrated Setup*\.

You must use Integrated Setup to set up OpsCenter with Explorer\.  You can't set up Explorer and OpsCenter programmatically\. For more information, see [Getting started with Systems Manager Explorer and OpsCenter](Explorer-setup.md)\. 

**To set up OpsCenter**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. On the OpsCenter home page, choose **Get started**\.

1. On the OpsCenter setup page, choose **Enable this option to have Explorer configure AWS Config and Amazon CloudWatch events to automatically create OpsItems based on commonly\-used rules and events**\. If you don't choose this option, OpsCenter remains disabled\.
**Note**  
Amazon EventBridge \(formerly Amazon CloudWatch Events\) provides all functionality of CloudWatch Events and some new features, such as custom event buses, third\-party event sources and schema registry\.

1. Choose **Enable OpsCenter**\.

After you enable OpsCenter, you can do the following from **Settings**:
+ Create CloudWatch alarms using the **Open CloudWatch console** button\. For more information, see [Configure CloudWatch alarms to create OpsItems](OpsCenter-create-OpsItems-from-CloudWatch-Alarms.md)\.
+ Enable operational insights\. For more information, see [Analyzing operational insights to reduce duplicate OpsItems](OpsCenter-working-operational-insights.md)\.
+ Enable AWS Security Hub findings alarms\. For more information, see [Security Hub](OpsCenter-integrate-with-security-hub.md)\.
+ Enable or disable the default EventBridge rules and custom rules that you have created\. You can also edit the category and severity for default and custom EventBridge rules\. The following EventBridge rules are available by default: 
  + **SSMOpsItems\-Autoscaling\-instance\-launch\-failure** – This rule creates OpsItems when the launch of the EC2 auto scaling instance is failed\. 
  + **SSMOpsItems\-Autoscaling\-instance\-termination\-failure** – This rule creates OpsItems when the termination of the EC2 auto scaling instance is failed\.
  + **SSMOpsItems\-EBS\-snapshot\-copy\-failed** – This rule creates OpsItems when the system failed to copy the Amazon Elastic Block Store \(Amazon EBS\) snapshot\. 
  + **SSMOpsItems\-EBS\-snapshot\-creation\-failed** – This rule creates OpsItems when the system failed to create the Amazon EBS snapshot\.
  + **SSMOpsItems\-EBS\-volume\-performance\-issue** – This rule creates OpsItems whenever there is a performance issue with Amazon EBS volume\. 
  + **SSMOpsItems\-EC2\-issue** – This rule creates OpsItems whenever there is an issue with EC2 instances\.
  + **SSMOpsItems\-EC2\-scheduled\-change** – This rule creates OpsItems for EC2 scheduled updates\.
  + **SSMOpsItems\-RDS\-issue** – This rule creates OpsItems whenever there is an issue with Amazon Relational Database Service \(Amazon RDS\)\.
  + **SSMOpsItems\-RDS\-scheduled\-change** – This rule creates OpsItems for Amazon RDS scheduled updates\.
  + **SSMOpsItems\-SSM\-maintenance\-window\-execution\-failed** – This rule creates OpsItems when the execution of the Systems Managermaintenance window is failed\. 
  + **SSMOpsItems\-SSM\-maintenance\-window\-execution\-timedout** – This rule creates OpsItems when the execution of the Systems Manager maintenance window is timeout\. 

  You can choose a rule to view, edit, and manage in EventBridge\. For more information, see [Configure EventBridge rules to create OpsItems](OpsCenter-automatically-create-OpsItems-2.md)