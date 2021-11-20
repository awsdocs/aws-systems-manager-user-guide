# Automating updates to SSM Agent<a name="ssm-agent-automatic-updates"></a>

AWS releases a new version of AWS Systems Manager Agent \(SSM Agent\) when we add or update Systems Manager capabilities\. If your instances use an older version of the agent, then you can't use the new capabilities or benefit from the updated capabilities\. For these reasons, we recommend that you automate the process of updating SSM Agent on your instances using any of the following methods\.


****  

| Method | Details | 
| --- | --- | 
|  One\-click automated update on all instances \(Recommended\)  |  You can configure all instances in your AWS account to automatically check for and download new versions of SSM Agent\. To do this, choose **Agent auto update** on the **Managed instances** page in the Systems Manager console, as described later in this topic\.   One\-click automated updates aren't supported for macOS Amazon Elastic Compute Cloud \(Amazon EC2\) instances\.   | 
|  Global or selective update  |  You can use State Manager, a capability of AWS Systems Manager, to create an association that automatically downloads and installs SSM Agent on your instances\. If you want to limit the disruption to your workloads, you can create a Systems Manager maintenance window to perform the installation during designated time periods\. Both methods allow you to create either a global update configuration for all of your instances or selectively choose which instances get updated\. For information about creating a State Manager association, see [Walkthrough: Automatically update SSM Agent \(CLI\)](sysman-state-cli.md)\. For information about using a maintenance window, see [Walkthrough: Create a maintenance window to update SSM Agent \(AWS CLI\)](mw-walkthrough-cli.md) and [Walkthrough: Create a maintenance window to automatically update SSM Agent \(console\)](mw-walkthrough-console.md)\.  | 
|  Global or selective update for new environments  |  If you're getting started with Systems Manager, we recommend that you use the **Update Systems Manager \(SSM\) Agent every two weeks** option in Quick Setup, a capability of AWS Systems Manager\. Quick Setup allows you to create either a global update configuration for all of your instances or selectively choose which instances get updated\. For more information, see [AWS Systems Manager Quick Setup](systems-manager-quick-setup.md)\.  | 

If you prefer to update SSM Agent on your instances manually, you can subscribe to notifications that AWS publishes when a new version of the agent is released\. For information, see [Subscribing to SSM Agent notifications](ssm-agent-subscribe-notifications.md)\. After you subscribe to notifications, you can use Run Command to manually update one or more instances with the latest version\. For more information, see [Update SSM Agent by using Run Command](rc-console.md#rc-console-agentexample)\.

## Automatically updating SSM Agent<a name="ssm-agent-automatic-updates-console"></a>

You can configure Systems Manager to automatically update SSM Agent on all Linux\-based and Windows\-based managed instances in your AWS account\. \(One\-click automated updates aren't supported for macOS EC2 instances\.\) If you turn on this option, then Systems Manager automatically checks every two weeks for a new version of the agent\. If there is a new version, then Systems Manager automatically updates the agent to the latest released version using the SSM document `AWS-UpdateSSMAgent`\. We encourage you to choose this option to ensure that your instances are always running the most up\-to\-date version of SSM Agent\. 

**Note**  
If you use a `yum` command to update SSM Agent on a managed instance after the agent has been installed or updated using the SSM document `AWS-UpdateSSMAgent`, you might see the following message: "Warning: RPMDB altered outside of yum\." This message is expected and can be safely ignored\.

**To automatically update SSM Agent**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the **Settings** tab, and then choose **Auto update SSM Agent** under **Agent auto update**\.

To change the version of SSM Agent your fleet updates to, choose **Edit** under **Agent auto update** on the **Settings** tab\. Then enter the version number of SSM Agent you want to update to in **Version** under **Parameters**\. If not specified, the agent updates to the latest version\. 

To stop automatically deploying updated versions of SSM Agent to all managed instances in your account, choose **Delete** under **Agent auto update** on the **Settings** tab\. This action deletes the State Manager association that automatically updates SSM Agent on your instances\.