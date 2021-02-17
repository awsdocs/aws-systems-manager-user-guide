# Editing default rules for OpsItems<a name="Explorer-using-editing-default-rules"></a>

When you complete Integrated Setup, the system enables more than a dozen rules in Amazon EventBridge\. These rules automatically create OpsItems in Systems Manager OpsCenter\. Systems Manager Explorer then displays aggregated information about the OpsItems\.

Each rule includes a preset **Category** and **Severity** value\. When the system creates OpsItems from an event, it automatically assigns the preset **Category** and **Severity**\.

**Important**  
Currently, you can't edit the **Category** and **Severity** values for default rules but you can edit these values on OpsItems created from the default rules\. 

![\[Default rules for creating OpsItems in Systems Manager Explorer\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/explorer-default-rules.png)

**To edit default rules for creating OpsItems**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Choose **Settings**\.

1. In the **OpsItems rules** section, choose **Edit**\.

1. Expand **CWE rules**\.

1. Clear the check box beside those rules that you don't want to use\.

1. Use the **Category** and **Severity** lists to change this information for a rule\. 

1. Choose **Save**\.

Your changes take effect the next time the system creates an OpsItem\.