# Creating OpsItems<a name="OpsCenter-creating-OpsItems"></a>

You can create OpsItems automatically based on Amazon CloudWatch alarms and Amazon EventBridge events\. When an alarm reaches the alarm state or an event is processed, the system creates an OpsItem in OpsCenter and provides detailed information about the alarm or the event in the OpsItem\. You can also create OpsItems manually\.

**Note**  
When you initially configure OpsCenter by using Integrated Setup, you enable EventBridge to automatically create OpsItems based on common rules\. The procedures in this section describe how to configure a specific alarm or event to create OpsItems\. For information about enabling default EventBridge rules for creating OpsItems by using Integrated Setup, see [Getting started with Systems Manager Explorer and OpsCenter](Explorer-setup.md)\.

**Topics**
+ [Configuring CloudWatch to create OpsItems from alarms](OpsCenter-create-OpsItems-from-CloudWatch-Alarms.md)
+ [Configuring EventBridge to automatically create OpsItems for specific events](OpsCenter-automatically-create-OpsItems-2.md)
+ [Configuring CloudWatch Application Insights for \.NET and SQL Server to automatically create OpsItems](OpsCenter-getting-started-user-CloudWatch-Application-Insights.md)
+ [Creating OpsItems manually](OpsCenter-manually-create-OpsItems.md)