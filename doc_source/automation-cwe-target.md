# Configuring Automation as a CloudWatch Events Target \(Optional\)<a name="automation-cwe-target"></a>

You can start an Automation workflow by specifying an Automation document as the target of an Amazon CloudWatch event\. You can start workflows according to a schedule, or when a specific AWS system event occurs\. For example, let's say you create an Automation document named *BootStrapInstances* that installs software on an instance when an instance starts\. To specify the *BootStrapInstances* document \(and corresponding workflow\) as a target of a CloudWatch event, you first create a new CloudWatch Events rule\. \(Here's an example rule: **Service name**: EC2, **Event Type**: EC2 Instance State\-change Notification, **Specific state\(s\)**: running, **Any instance**\.\) Then you use the following procedure to specify the *BootStrapInstances* document as the target of the event\. When a new instances starts, the system runs the workflow and installs software\.

For information about creating Automation documents, see [Working with Automation Documents](automation-documents.md)\.

Use the following procedure to configure an Automation workflow as the target of a CloudWatch event\.

**To configure Automation as a target of a CloudWatch event**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Events**, and then choose **Create rule**\.

1. Choose **Event Pattern** or **Schedule**\. **Event Pattern** lets you build a rule that generates events for specific actions in AWS services\. **Schedule** lets you build a rule that generates events according to a schedule that you specify by using the cron format\.

1. Choose the remaining options for the rule you want to create, and then choose **Add target**\.

1. In the **Select target type** list, choose **SSM Automation**\. 

1. In the **Document** list, choose an SSM document to run when your target is invoked\.

1. Expand **Configure document version**, and choose a version\. $DEFAULT was explicitly set as the default document in Systems Manager\. You can choose a specific version, or use the latest version\.

1. Expand **Configure automation parameter\(s\)**, and either keep the default parameter values \(if available\) or enter your own values\. 
**Note**  
Required parameters have an asterisk \(\*\) next to the parameter name\. To create a target, you must specify a value for each required parameter\. If you don't, the system creates the rule, but it won't run\.

1. In the permissions section, choose an option\. CloudWatch uses the role to start the Automation workflow\. 

1. Choose **Configure details** and complete the wizard\.