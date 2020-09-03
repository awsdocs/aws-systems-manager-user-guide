# Creating OpsItems<a name="OpsCenter-creating-OpsItems"></a>

You can create OpsItems automatically based on Amazon CloudWatch Events and CloudWatch alarms\. When an event is triggered or an alarm reaches the alarm state, CloudWatch creates an OpsItem in OpsCenter and provides detailed information about the event or the alarm in the OpsItem **Operational data** section\. You can also create OpsItems manually\.

**Note**  
When you initially configure OpsCenter by using Integrated Setup, you enable Amazon CloudWatch Events to automatically create OpsItems based on common rules\. The procedures in this section describe how to configure a specific event or alarm to create OpsItems\. For information about enabling default CloudWatch Events rules for creating OpsItems by using Integrated Setup, see [Getting started with Systems Manager Explorer and OpsCenter](Explorer-setup.md)\.

**Topics**
+ [Configuring CloudWatch to automatically create OpsItems for specific alarms](OpsCenter-automatically-create-OpsItems-1.md)
+ [Configuring CloudWatch Events to automatically create OpsItems for specific events](OpsCenter-automatically-create-OpsItems-2.md)
+ [Configuring CloudWatch Application Insights for \.NET and SQL Server to automatically create OpsItems](OpsCenter-getting-started-user-CloudWatch-Application-Insights.md)
+ [Creating OpsItems manually](OpsCenter-manually-create-OpsItems.md)