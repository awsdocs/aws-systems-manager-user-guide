# Disable or enable a maintenance window<a name="sysman-maintenance-disable"></a>

You can disable or enable a maintenance window in Maintenance Windows, a capability of AWS Systems Manager\. You can choose one maintenance window at a time to either disable or enable the maintenance window from running\. You can also select multiple or all maintenance windows to enable and disable\.

This section describes how to disable or enable a maintenance window by using the Systems Manager console\. For examples of how to do this by using the AWS Command Line Interface \(AWS CLI\), see [Tutorial: Update a maintenance window \(AWS CLI\)](maintenance-windows-cli-tutorials-update.md)\. 

**Topics**
+ [Disable a maintenance window \(console\)](#sysman-maintenance-disable-mw)
+ [Enable a maintenance window \(console\)](#sysman-maintenance-enable-mw)

## Disable a maintenance window \(console\)<a name="sysman-maintenance-disable-mw"></a>

You can disable a maintenance window to pause a task for a specified period, and it will remain available to enable again later\.

**To disable a maintenance window**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\. 

1. Using the check box next to the maintenance window that you want to disable, select one or more maintenance windows\.

1. Choose **Disable maintenance window** in the **Actions** menu\. The system prompts you to confirm your actions\. 

## Enable a maintenance window \(console\)<a name="sysman-maintenance-enable-mw"></a>

You can enable a maintenance window to resume a task\.

**To enable a maintenance window**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\. 

1. Using the check box next to the maintenance window that you want to enable, select one or more maintenance windows\.

1. Choose **Enable maintenance window** in the **Actions** menu\. The system prompts you to confirm your actions\. 