# Create OpsItems<a name="OpsCenter-create-OpsItems"></a>

After you set up OpsCenter, a capability of AWS Systems Manager, and integrate it with your AWS services, your AWS services automatically create OpsItems based on default rules, events, or alarms\. 

You can view the statuses and severity levels of default Amazon EventBridge rules\. If required, you can create or edit these rules from Amazon EventBridge\. You can also view alarms from Amazon CloudWatch, and create or edit alarms\. Using rules and alarms, you can configure events for which you want to generate OpsItems automatically\.

When the system creates an OpsItem, it's in the **Open** status\. You can change the status to **In progress** when you start investigation of the OpsItem and to **Resolved** after you remediate the OpsItem\. For more information about how to configure alarms and rules in AWS services to create OpsItems and how to create OpsItems manually, see the following topics\. 

**Topics**
+ [Configure EventBridge rules to create OpsItems](OpsCenter-automatically-create-OpsItems-2.md)
+ [Configure CloudWatch alarms to create OpsItems](OpsCenter-create-OpsItems-from-CloudWatch-Alarms.md)
+ [Create OpsItems manually](OpsCenter-manually-create-OpsItems.md)