# Quick Setup Host Management<a name="quick-setup-host-management"></a>

Use Quick Setup, a capability of AWS Systems Manager, to quickly configure required security roles and commonly used Systems Manager capabilities on your Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. You can use Quick Setup in an individual account or across multiple accounts and AWS Regions by integrating with AWS Organizations\. These capabilities help you manage and monitor the health of your instances while providing the minimum required permissions to get started\. 

If you're unfamiliar with Systems Manager services and features, we recommend that you review the *AWS Systems Manager User Guide* before creating a configuration with Quick Setup\. For more information about Systems Manager, see [What is AWS Systems Manager?](what-is-systems-manager.md)\.

**Important**  
Quick Setup might not be the right tool to use for EC2 management if either of the following applies to you:  
You’re trying to create an EC2 instance for the first time to try out AWS capabilities\.
You’re still new to EC2 instance management\.
Instead, we recommend that you explore the following content:   
[Getting Started with Amazon EC2](http://aws.amazon.com/ec2/getting-started)
[Launch an instance using the new launch instance wizard](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-launch-instance-wizard.html) in the *Amazon EC2 User Guide for Linux Instances*
[Launch an instance using the new launch instance wizard](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-launch-instance-wizard.html) in the *Amazon EC2 User Guide for Windows Instances*
[Tutorial: Get started with Amazon EC2 Linux instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) in the *Amazon EC2 User Guide for Linux Instances*
If you’re already familiar with EC2 instance management and want to streamline configuration and management for multiple EC2 instances, use Quick Setup\. Whether your organization has dozens, thousands, or millions of EC2 instances, use the following Quick Setup procedure to configure multiple options for them, all at once\.

To set up host management, perform the following tasks in the AWS Systems Manager Quick Setup console\.

**To set up host management with Quick Setup**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Quick Setup**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Quick Setup** in the navigation pane\.

1. Choose **Create**\.

1. Choose **Host management**, and then choose **Next**\.

1. In the **Configuration options** section, choose the options that you want to allow for your configuration\.
   + **Update Systems Manager \(SSM\) Agent every two weeks** – Enables Systems Manager to check every two weeks for a new version of the agent\. If there is a new version, then Systems Manager automatically updates the agent on your managed node to the latest released version\. 

     We encourage you to choose this option to ensure that your nodes are always running the most up\-to\-date version of SSM Agent\. For more information about SSM Agent, including information about how to manually install the agent, see [Working with SSM Agent](ssm-agent.md)\.
   + **Collect inventory from your instances every 30 minutes** – Enables Quick Setup to configure collection of the following types of metadata:
     + **AWS components** – EC2 driver, agents, versions, and more\.
     + **Applications** – Application names, publishers, versions, and more\.
     + **Node details** – System name, operating system \(OS\) name, OS version, last boot, DNS, domain, work group, OS architecture, and more\.
     + **Network configuration** – IP address, MAC address, DNS, gateway, subnet mask, and more\. 
     + **Services** – Name, display name, status, dependent services, service type, start type, and more \(Windows Server nodes only\)\.
     + **Windows roles** – Name, display name, path, feature type, installed state, and more \(Windows Server nodes only\)\.
     + **Windows updates** – Hotfix ID, installed by, installed date, and more \(Windows Server nodes only\)\.

     For more information about Inventory, a capability of AWS Systems Manager, see [AWS Systems Manager Inventory](systems-manager-inventory.md)\.
**Note**  
The **Inventory collection** option can take up to 10 minutes to complete, even if you only selected a few nodes\.
   + **Scan instances for missing patches daily** – Enables Patch Manager, a capability of Systems Manager, to scan your nodes daily and generate a report in the **Compliance** page\. The report shows how many nodes are patch\-compliant according to the *default patch baseline*\. The report includes a list of each node and its compliance status\. 

     For information about patching operations and patch baselines, see [AWS Systems Manager Patch Manager](patch-manager.md)\. 

     For information about patch compliance, see the Systems Manager [Compliance](https://console.aws.amazon.com/systems-manager/compliance) page\.

     For information about patching managed nodes in multiple accounts and Regions in one configuration, see [Using Quick Setup patch policies](patch-manager-policies.md) and [Automate organization\-wide patching using a Quick Setup patch policyAutomate organization\-wide patching](quick-setup-patch-manager.md)\.
**Important**  
Systems Manager supports several methods for scanning managed nodes for patch compliance\. If you implement more than one of these methods at a time, the patch compliance information you see is always the result of the most recent scan\. Results from previous scans are overwritten\. If the scanning methods use different patch baselines, with different approval rules, the patch compliance information can change unexpectedly\. For more information, see [Avoiding unintentional patch compliance data overwrites](patch-manager-compliance-data-overwrites.md)\.
   + **Install and configure the CloudWatch agent** – Installs the basic configuration of the unified CloudWatch agent on your Amazon EC2 instances\. The agent collects metrics and log files from your instances for Amazon CloudWatch\. This information is consolidated so you can quickly determine the health of your instances\. For more information about the CloudWatch agent basic configuration, see [CloudWatch agent predefined metric sets](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create-cloudwatch-agent-configuration-file-wizard.html#cloudwatch-agent-preset-metrics)\. There might be added cost\. For more information, see [Amazon CloudWatch pricing](http://aws.amazon.com/cloudwatch/pricing/)\.
   + **Update the CloudWatch agent once every 30 days** – Enables Systems Manager to check every 30 days for a new version of the CloudWatch agent\. If there is a new version, Systems Manager updates the agent on your instance\. We encourage you to choose this option to ensure that your instances are always running the most up\-to\-date version of the CloudWatch agent\.

1. In the **Targets** section, choose whether to set up host management for your **Entire organization**, **Custom** organizational units \(OUs\), or the **Current account** you're signed in to:
**Note**  
You can't create multiple Quick Setup Host Management configurations that target the same AWS Region\.
   + **Entire organization** – In the **Instance profile options** section, choose whether you want to add the required IAM policies to the existing instance profiles attached to your instances, or to allow Quick Setup to create the IAM policies and instance profiles with the permissions needed for the configuration you choose\.
**Note**  
 The **Entire organization** option is only available if you're configuring host management from your organization's management account\.
   + **Custom** – In the **Target OUs** section, select the OUs where you want to set up host management\. Next, in the **Target Regions** section, select the Regions where you want to set up host management\. Then, in the **Instance profile options** section, choose whether you want to add the required IAM policies to the existing instance profiles attached to your instances, or to allow Quick Setup to create the IAM policies and instance profiles with the permissions needed for the configuration you choose\.
   + **Current account** – Select **Current Region** or **Choose Regions**\. Next, select how you want to target instances\. Then, if you selected **Current Region**, continue to step 7\. If you selected **Choose Regions** choose the **Target Regions** where you want to set up host management and then continue to step 7\.

1. Choose **Create**\.