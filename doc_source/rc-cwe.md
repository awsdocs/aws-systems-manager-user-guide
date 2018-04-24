# Configuring CloudWatch Events for Run Command<a name="rc-cwe"></a>

Use Amazon CloudWatch Events to log command execution status changes\. You can create a rule that runs whenever there is a state transition, or when there is a transition to one or more states that are of interest\. 

You can also specify Run Command as a target action when a CloudWatch event occurs\. For example, say a CloudWatch event is triggered that an instance in an Auto Scaling group is about to terminate\. You can configure CloudWatch so the target of that event is a Run Command script that captures the log files from the instance before it is terminated\. You can also configure a Run Command action when a new instance is created in an Auto Scaling group\. For example, when CloudWatch receives the instance\-created event, Run Command could enable the web server role or install software on the instance\.
+ [Configuring CloudWatch Events for Run Command](#rc-cwe-logging)
+ [Configure Run Command as a CloudWatch Events Target](#rc-cwe-target)

## Configuring CloudWatch Events for Run Command<a name="rc-cwe-logging"></a>

You can configure CloudWatch Events to notify you of Run Command status changes, or a status change for a specific command invocation\. Use the following procedure to configure CloudWatch Events to send notification about Run Command\. 

**To configure CloudWatch Events for Run Command**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Events**, and then choose **Create rule**\.

1. Under **Event Source**, verify that **Event Pattern** is selected\.

1. In the **Service Name** field, choose **EC2 Simple Systems Manager \(SSM\)**

1. In the **Event Type** field, choose **Run Command**\.

1. Choose the detail types and statuses for which you want to receive notifications, and then choose **Add targets**\.

1. In the **Select target type** list, choose a target type\. For information about the different types of targets, see the corresponding AWS Help documentation\. 

1. Choose **Configure details**\.

1. Specify the rule details, and then choose **Create rule**\.

## Configure Run Command as a CloudWatch Events Target<a name="rc-cwe-target"></a>

Use the following procedure to configure a Run Command action as the target of a CloudWatch event\.

**To configure Run Command as a target of a CloudWatch event**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Events**, and then either choose to create a new rule or edit an existing rule\.

1. After specifying or verifying the details of the rule, choose **Add target**\.

1. In the **Select target type** list, choose **SSM Run Command**\. 

1. In the **Document** list, choose an SSM document\. The document determines the type of actions Run Command can perform on your instances\.
**Note**  
Verify that the document you choose can run on the instance operating system\. Some documents run only on Windows or only on Linux operating systems\. For more information about SSM Documents, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

1. In the **Target key** field, specify either InstanceIds or tag:*EC2\_tag\_name*\. Here are some examples of a **Target key** that uses an EC2 tag: tag:production and tag:server\-role\.

1. In the **Target value\(s\)** field, if you chose InstanceIds in the previous step, specify one or more instance IDs separated by commas\. If you chose tag:*EC2\_tag\_name* in the previous step, specify one or more tag values\. After you type the value, for example web\-server or database, choose **Add**\.

1. In the **Configure parameter\(s\)** section, choose an option and then complete any fields populated by your choice\. Use the hover text for more information about the options\. For more information about the parameter fields for your document, see [Running Commands Using Systems Manager Run Command](run-command.md) and choose the procedure for your document\.

1. In the permissions section, choose **Create a new role for this specific resource** to create a new role with the required instance profile role for Run Command\. Or, choose **Use existing role**\. For more information about roles required for Run Command, see [Configuring Access to Systems Manager](systems-manager-access.md)\.

1. Choose **Configure details** and complete the wizard\.