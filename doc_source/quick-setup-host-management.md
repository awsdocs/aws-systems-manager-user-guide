# Quick Setup Host Management<a name="quick-setup-host-management"></a>

Use Quick Setup, a capability of AWS Systems Manager, to quickly configure required security roles and commonly used Systems Manager capabilities on your Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. You can use Quick Setup in an individual account or across multiple accounts and AWS Regions by integrating with AWS Organizations\. These capabilities help you manage and monitor the health of your instances while providing the minimum required permissions to get started\. If you're unfamiliar with Systems Manager services and features, we recommend that you review the *AWS Systems Manager User Guide* before creating a configuration with Quick Setup\. For more information about Systems Manager, see [What is AWS Systems Manager?](what-is-systems-manager.md)\.

**Note**  
You can't create multiple Quick Setup Host Management configurations that target the same AWS Region\.

Organization Quick Setup is available in the following AWS Regions:
+ US East \(Ohio\)
+ US East \(N\. Virginia\)
+ US West \(N\. California\)
+ US West \(Oregon\)
+ Asia Pacific \(Mumbai\)
+ Asia Pacific \(Seoul\)
+ Asia Pacific \(Singapore\)
+ Asia Pacific \(Sydney\)
+ Asia Pacific \(Tokyo\)
+ Canada \(Central\)
+ Europe \(Frankfurt\)
+ Europe \(Ireland\)
+ Europe \(London\)
+ Europe \(Paris\)
+ South America \(São Paulo\)

Quick Setup for individual AWS accounts is available in all AWS Regions where Systems Manager is supported\. For a list of supported Regions, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

## Setting up host management<a name="quick-setup-host-management-setup"></a>

To set up host management, perform the following tasks in the AWS Systems Manager Quick Setup console\.

**To set up host management with Quick Setup**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Quick Setup**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Quick Setup** in the navigation pane\.

1. Choose **Create**\.

1. Choose **Host management**, and then choose **Next**\.

1. In the **Configuration options** section, choose the options that you want to allow for your configuration\.

   If you choose the **Update Systems Manager \(SSM\) Agent every two weeks** option, then Systems Manager automatically checks every two weeks for a new version of the agent\. If there is a new version, then Systems Manager automatically updates the agent on your instance to the latest released version\. We encourage you to choose this option to ensure that your instances are always running the most up\-to\-date version of SSM Agent\. For more information about SSM Agent, including information about how to manually install the agent, see [Working with SSM Agent](ssm-agent.md)\.

   If you choose the **Collect inventory from your instances every 30 minutes** option, Quick Setup configures collection for the following types of metadata:
   + **AWS components** – EC2 driver, agents, versions, and more\.
   + **Applications** – Application names, publishers, versions, and more\.
   + **Instance details** – System name, operating system \(OS\) name, OS version, last boot, DNS, domain, work group, OS architecture, and more\.
   + **Network configuration** – IP address, MAC address, DNS, gateway, subnet mask, and more\. 
   + **Services** – Name, display name, status, dependent services, service type, start type, and more \(Windows Server instances only\)\.
   + **Windows roles** – Name, display name, path, feature type, installed state, and more \(Windows Server instances only\)\.
   + **Windows updates** – Hotfix ID, installed by, installed date, and more \(Windows Server instances only\)\.

   For more information about Inventory, a capability of AWS Systems Manager, see [AWS Systems Manager Inventory](systems-manager-inventory.md)\.

   If you choose the **Scan instances for missing patches daily** option, then Systems Manager uses Patch Manager to scan your instances each day and generate a simple report in the **Compliance** page\. Patch Manager is a capability of AWS Systems Manager\. The report shows how many instances are patch\-compliant according to the *default patch baseline*\. The report includes a list of each instance and its compliance status\. You can navigate this list to see details about noncompliant instances\. For more information about patching operations and patch baselines, see [AWS Systems Manager Patch Manager](systems-manager-patch.md)\. To view compliance information, see the Systems Manager [Compliance](https://console.aws.amazon.com/systems-manager/compliance) page\.

   If you choose the **Install and configure the CloudWatch agent** option, the CloudWatch agent is installed on your Amazon EC2 instances\. The agent collects metrics and log files from your instances for Amazon CloudWatch\. This information is consolidated so you can quickly determine the health of your instances\. For more information, see [Collecting metrics and logs from EC2 instances and on\-premises servers with the CloudWatch Agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html)\. There might be added cost\. For more information, see [Amazon CloudWatch pricing](https://aws.amazon.com/cloudwatch/pricing/)\.

   If you choose the **Update the CloudWatch agent once every 30 days** option, then Systems Manager automatically checks every 30 days for a new version of the CloudWatch agent\. If there is a new version, then Systems Manager automatically updates the agent on your instance to the latest released version\. We encourage you to choose this option to ensure that your instances are always running the most up\-to\-date version of the CloudWatch agent\.

1. In the **Targets** section, choose whether to set up host management for your entire organization, some of your organizational units \(OUs\), or the account you're logged in to\.

   If you choose **Entire organization**, continue to step 8\. 

   If you choose **Custom**, continue to step 7\.

1. In the **Target OUs** section, select the check boxes of the OUs and Regions where you want to set up host management\.

1. In the **Instance profile options** section, choose whether you want to add the required IAM policies to the existing instance profiles attached to your instances, or to allow Quick Setup to create the IAM policies and instance profiles with the permissions needed for the configuration you choose\.

1. Choose **Create**\.

**Note**  
You can edit Quick Setup configurations for your account any time\. To edit an **Organization** Quick Setup configuration, the **Configuration status** must be **Success** or **Failed**\. Before editing a Quick Setup configuration, we recommend that you learn how to change configurations by using the Quick Setup Results page\. For more information, see [Working with Quick Setup Results](#quick-setup-results)\.

## Working with Quick Setup Results<a name="quick-setup-results"></a>

Systems Manager displays the results of Quick Setup in a separate card for each option you selected\. 

![\[The results of AWS Quick Setup.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/quick-setup-results-page.png)

For each option you selected, Systems Manager creates and immediately runs a State Manager association\. The **Configuration status** field shows **Success** when the association successfully runs on *all* selected or targeted instances\. If one association fails to run, **Configuration status** is **Failed**\. If an association is processing, **Configuration status** is **Pending**\. To update **Configuration status** for **Pending** items, refresh your browser\.

**Note**  
The **Inventory collection** option can take up to 10 minutes to complete, even if you only selected a few instances\.
+ A **Configuration status** of **Not configured** means that you didn't choose the option in Quick Setup\. If you see this status for **Managed instances**, it means that you didn't choose a role in the **Role selection** list\. You can run Quick Setup again, as described in this topic, and choose a role\.
+ To edit the association for a Quick Setup option, choose the **Edit** button in the option card\. If you edit the association, * don't* choose a different SSM document in the **Edit Association** page\. If you choose a different SSM document, the option becomes unavailable in Quick Setup\. Change only the parameters and targets of the association\. When you save your changes to the association, State Manager automatically runs the association\. For more information about associations, see [Working with associations in Systems Manager](systems-manager-associations.md)\.
+ To view compliance data for your Quick Setup, choose the **View compliance summary in Explorer** button\. This opens the Explorer dashboard\. For more information about Explorer, a capability of AWS Systems Manager, see [AWS Systems Manager Explorer](Explorer.md)\.
+ When viewing Quick Setup results while signed in to the management account for your AWS Organizations organization, you see a summary of the results for your **Organization** Quick Setup in the **Organization** tab\. This view shows the **Applied configuration** for your Quick Setup, allows you to filter results, shows the **Deployment** and **Association** statuses for your targets, and allows you to view more details for individual targets in the **Quick Setup deployments** card\.
+ **Deployment status** refers to the state of a target or stack instance\. In other words, this status indicates whether AWS CloudFormation successfully deployed your configuration options\. **Deployment status** doesn't evaluate the status of the associations created in your Quick Setup\.
+ **Association status** refers to the state of all associations created by your Quick Setup within a target or stack instance\. All associations must successfully run, otherwise the status for the target is **Failed**\.

## Troubleshooting Quick Setup Results<a name="quick-setup-results-troubleshooting"></a>

If a Quick Setup card shows **Not configured**, you might have missed an option on the Quick Setup page\. As a first step in troubleshooting this problem, choose the **Edit all** button at the top of the Quick Setup results page and review your selections\. If you missed one or more options, you can select the options and then choose **Reset** to configure those options\.

If you still see a problem with one or more Quick Setup results cards, then use the following procedure to troubleshoot the issue\.

**Note**  
To investigate **Failed** associations for an **Organization** Quick Setup, you must sign in to the account with the failed association and use the following procedure\. The **Association ID** isn't a hyperlink to the target account when viewing results from the management account\.

**To troubleshoot a failed Quick Setup configuration**

1. In the Quick Setup results page, choose **View Details** in the card with a **Configuration status** of **Failed**\.  
![\[Choosing the View Details button in a Quick Setup card\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/quick-setup-troubleshooting-1.png)

1. In the **Association ID** page, choose the **Execution history** tab\.

1. Under **Execution ID**, choose the association execution that failed\.  
![\[Choosing an association execution ID\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/quick-setup-troubleshooting-2.png)

1. The **Association execution targets** page lists all of the instances where the association ran\. Choose the **Output** button for an execution that failed to run\.  
![\[Choosing the Output button for an association execution that failed to run\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/quick-setup-troubleshooting-3.png)

1. In the **Output** page, choose **Step \- Output** to view the error message for that step in the command execution\. Each step can display a different error message\. Review the error messages for all steps to help troubleshoot the issue\.  
![\[Choosing Step Output for an association execution that failed to run\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/quick-setup-troubleshooting-4.png)

If viewing the step output doesn't solve the problem, then you can recreate the association to see if the problem persists\. To recreate the association, you must first delete all associations that already exist for the Quick Setup option\. You can delete an association by using the **Delete** button in the Quick Setup results card\. You can also delete the association by choosing **State Manager** in the navigation pane\. After you delete the association, run Quick Setup again to see if the problem persists\.

## Editing and deleting your configuration<a name="quick-setup-edit-delete"></a>

You can run Quick Setup again by choosing the **Edit all** button on the Quick Setup results page\. You can select or clear options\. If you clear an option, Systems Manager doesn't delete the association\. For example, if you previously selected the **Collect inventory from your instances every 30 minutes** option, and you clear the option when you run Quick Setup again, the original association still exists\. You must either delete the association from the Quick Setup results page or from the **State Manager** page\.

**Important**  
If you want to run Quick Setup on new instances, we recommend that you choose the new instances and also choose all of the instances on which you previously ran Quick Setup\. By choosing all of the instances on which you previously ran Quick Setup, you synchronize the association executions for all of the instances\. This synchronizes status and compliance reporting\. We also recommend that you target instances by using tags\. New instances with the specified tags are *automatically* added to Quick Setup associations\. This means that they automatically display status in the Quick Setup results and in the **Compliance** page\.

If you want to delete your Quick Setup from a specific Region, you must delete the associations from the Quick Setup results page or from the **State Manager** page in that Region\.