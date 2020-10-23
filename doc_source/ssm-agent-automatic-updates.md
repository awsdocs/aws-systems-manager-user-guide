# Automating updates to SSM Agent<a name="ssm-agent-automatic-updates"></a>

AWS releases a new version of SSM Agent when we add or update Systems Manager capabilities\. If your instances use an older version of the agent, then you can't use the new capabilities or benefit from the updated capabilities\. For these reasons, we recommend that you automate the process of updating SSM Agent on your instances using any of the following methods\.


****  

| Method | Details | 
| --- | --- | 
|  One\-click automated update on all instances \(Recommended\)  |  You can configure all instances in your AWS account to automatically check for and download new versions of SSM Agent\. To do this, choose **Agent auto update** on the **Managed instances** page in the AWS Systems Manager console, as described later in this topic\.  | 
|  Global or selective update  |  You can use State Manager to create an association that automatically downloads and installs SSM Agent on your instances\. If you want to limit the disruption to your workloads, you can create a Systems Manager maintenance window to perform the installation during designated time periods\. Both methods enable you to create either a global update configuration for all of your instances or selectively choose which instances get updated\. For information about creating a State Manager association, see [Automatically update SSM Agent \(CLI\)](sysman-state-cli.md)\. For information about using a maintenance window, see [Automatically Update SSM Agent \(AWS CLI\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/mw-walkthrough-cli.html) and [Automatically Update SSM Agent \(Console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/mw-walkthrough-console.html)\.   | 
|  Global or selective update for new environments  |  If you are getting started with Systems Manager, we recommend that you use the **Update Systems Manager \(SSM\) Agent every two weeks** option in Systems Manager Quick Setup\. Quick Setup enables you to create either a global update configuration for all of your instances or selectively choose which instances get updated\. For more information, see [AWS Systems Manager Quick Setup](systems-manager-quick-setup.md)\.  | 

If you prefer to update SSM Agent on your instances manually, you can subscribe to notifications that AWS publishes when a new version of the agent is released\. For information, see [Subscribing to SSM Agent notifications](ssm-agent-subscribe-notifications.md)\. After you subscribe to notifications, you can use Run Command to manually update one or more instances with the latest version\. For more information, see [Update SSM Agent by using Run Command](rc-console.md#rc-console-agentexample)\.

## Automatically updating SSM Agent<a name="ssm-agent-automatic-updates-console"></a>

You can configure Systems Manager to automatically update SSM Agent on all managed instances in your AWS account\. If you enable this option, then Systems Manager automatically checks every two weeks for a new version of the agent\. If there is a new version, then Systems Manager automatically updates the agent to the latest released version using the SSM document `AWS-UpdateSSMAgent`\. We encourage you to choose this option to ensure that your instances are always running the most up\-to\-date version of SSM Agent\. 

**Note**  
If you use a `yum` command to update SSM Agent on a managed instance after the agent has been installed or updated using the SSM document `AWS-UpdateSSMAgent`, you might see the following message: "Warning: RPMDB altered outside of yum\." This message is expected and can be safely ignored\.

**To automatically update SSM Agent**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed instances**\.

1. Choose **Agent auto update**\.

To stop automatically deploying updated versions of SSM Agent to all managed instances in your account, choose **Delete** on the **Managed instances** page\. This action deletes the State Manager association that automatically updates SSM Agent on your instances\.