# Creating OpsItems<a name="OpsCenter-creating-OpsItems"></a>

AWS Systems Manager automatically creates operational work items \(OpsItems\) in OpsCenter for some AWS services and Systems Manager capabilities, if those services and capabilities are enabled\. You can also configure the system to create OpsItems automatically based on Amazon CloudWatch alarms and Amazon EventBridge events\. When an alarm reaches the alarm state or an event is processed, the system creates an OpsItem in OpsCenter and provides detailed information about the alarm or the event in the OpsItem\. You can also create OpsItems manually\.

**Note**  
When you initially configure OpsCenter by using Integrated Setup, you allow EventBridge to automatically create OpsItems based on common rules\. The procedures in this section describe how to configure a specific alarm or event to create OpsItems\. For information about allowing default EventBridge rules for creating OpsItems by using Integrated Setup, see [Getting started with Systems Manager Explorer and OpsCenter](Explorer-setup.md)\.

**Topics**
+ [Services and capabilities that automatically create OpsItems](OpsCenter-create-OpsItems-services-that-automatically-create.md)
+ [Configuring CloudWatch to create OpsItems from alarms](OpsCenter-create-OpsItems-from-CloudWatch-Alarms.md)
+ [Configuring EventBridge to automatically create OpsItems for specific events](OpsCenter-automatically-create-OpsItems-2.md)
+ [Configuring CloudWatch Application Insights for \.NET and SQL Server to automatically create OpsItems](OpsCenter-getting-started-user-CloudWatch-Application-Insights.md)
+ [Creating OpsItems manually](OpsCenter-manually-create-OpsItems.md)