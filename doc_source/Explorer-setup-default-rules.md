# Enabling default rules<a name="Explorer-setup-default-rules"></a>

Integrated Setup automatically configures the following default rules in EventBridge\. These rules create OpsItems in Systems Manager OpsCenter\. If you don't want EventBridge to create OpsItems for the following events, then clear this option in Integrated Setup\. If you prefer, you can specify OpsCenter as the target of specific EventBridge events\. For more information, see [Configuring EventBridge to automatically create OpsItems for specific events](OpsCenter-automatically-create-OpsItems-2.md)\. You can also disable the default rules at any time on the **Settings** page\.

**Important**  
Currently, you can't edit the **Category** and **Severity** values for default rules but you can edit these values on OpsItems created from the default rules\. 

![\[Default rules for creating OpsItems in Systems Manager Explorer\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/explorer-default-rules.png)