# Configure CloudWatch alarms to create OpsItems<a name="OpsCenter-create-OpsItems-from-CloudWatch-Alarms"></a>

During the integrated setup of OpsCenter, a capability of AWS Systems Manager, you enable Amazon CloudWatch to automatically create OpsItems based on common alarms\. You can create an alarm or edit an existing alarm to create OpsItems in OpsCenter\. 

CloudWatch creates a new service\-linked role in AWS Identity and Access Management \(IAM\) when you configure an alarm to create OpsItems\. The new role is named `AWSServiceRoleForCloudWatchAlarms_ActionSSM`\. For more information about CloudWatch service\-linked roles, see [Using service\-linked roles for CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/using-service-linked-roles.html) in the *Amazon CloudWatch User Guide*\. 

When a CloudWatch alarm generates an OpsItem, the OpsItem displays **CloudWatch alarm \- '*alarm\_name*' is in ALARM state**\. 

To view details about a specific OpsItem, choose the OpsItem and then choose the **Related resource details** tab\. You can manually edit OpsItems to change details, such as the severity or category\. However, when you edit the severity or the category of an alarm, Systems Manager can't update the severity or category of OpsItems that are already created from the alarm\. If an alarm created an OpsItem and if you specified a deduplication string, the alarm won't create additional OpsItems even if you edit the alarm in CloudWatch\. If the OpsItem is resolved in OpsCenter, CloudWatch will create a new OpsItem\.

For more information about configuring CloudWatch alarms, see the following topics\.

**Topics**
+ [Configuring a CloudWatch alarm to create OpsItems \(console\)](OpsCenter-creating-or-editing-existing-alarm-console.md)
+ [Configuring an existing CloudWatch alarm to create OpsItems \(programmatically\)](OpsCenter-configuring-an-existing-alarm-programmatically.md)