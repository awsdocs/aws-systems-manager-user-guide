# Automate organization\-wide patching using a Quick Setup patch policy<a name="quick-setup-patch-manager"></a>

With Quick Setup, a capability of AWS Systems Manager, you can create patch policies powered by Patch Manager\. A patch policy defines the schedule and baseline to use when automatically patching your Amazon Elastic Compute Cloud \(Amazon EC2\) instances and other managed nodes\. Using a single patch policy configuration, you can define patching for all accounts in multiple AWS Regions in your organization, for only the accounts and Regions you choose, or for a single account\-Region pair\. For more information about patch policies, see [Introducing patch policies](patch-policies-about.md)\.

**Prerequisite**  
To define a patch policy for a node using Quick Setup, the node must be a *managed node*\. For more information about managing your nodes, see [Setting up AWS Systems Manager](systems-manager-setting-up.md)\.

**Important**  
Systems Manager supports several methods for scanning managed nodes for patch compliance\. If you implement more than one of these methods at a time, the patch compliance information you see is always the result of the most recent scan\. Results from previous scans are overwritten\. If the scanning methods use different patch baselines, with different approval rules, the patch compliance information can change unexpectedly\. For more information, see [Avoiding unintentional patch compliance data overwrites](avoid-patch-compliance-overwrites.md)\.

## Supported Regions for patch policy configurations<a name="patch-policies-supported-regions"></a>

Patch policy configurations in Quick Setup are currently supported in the following Regions:
+ US East \(Ohio\) \(us\-east\-2\)
+ US East \(N\. Virginia\) \(us\-east\-1\)
+ US West \(N\. California\) \(us\-west\-1\)
+ US West \(Oregon\) \(us\-west\-2\)
+ Asia Pacific \(Mumbai\) \(ap\-south\-1\)
+ Asia Pacific \(Seoul\) \(ap\-northeast\-2\)
+ Asia Pacific \(Singapore\) \(ap\-southeast\-1\)
+ Asia Pacific \(Sydney\) \(ap\-southeast\-2\)
+ Asia Pacific \(Tokyo\) \(ap\-northeast\-1\)
+ Canada \(Central\) \(ca\-central\-1\)
+ Europe \(Frankfurt\) \(eu\-central\-1\)
+ Europe \(Ireland\) \(eu\-west\-1\)
+ Europe \(London\) \(eu\-west\-2\)
+ Europe \(Paris\) \(eu\-west\-3\)
+ Europe \(Stockholm\) \(eu\-north\-1\)
+ South America \(São Paulo\) \(sa\-east\-1\)

## Creating a patch policy<a name="create-patch-policy"></a>

To create a patch policy, perform the following tasks in the Systems Manager console\.

**To create a patch policy with Quick Setup**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

   If you are setting up patching for an organization, make sure you are signed in to the management account for the organization\. You can't set up the policy using the delegated administrator account or a member account\.

1. In the navigation pane, choose **Quick Setup**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Quick Setup** in the navigation pane\.

1. On the **Patch Manager** card, choose **Create**\. 

1. For **Configuration name**, enter a name to help identify the patch policy\.

1. In the **Scanning and installation** section, under **Patch operation**, choose whether the patch policy will **Scan** the specified targets or **Scan and install** patches on the specified targets\.

1. Under **Scanning schedule**, choose **Use recommended defaults** or **Custom scan schedule**\. The default scan schedule will scan your targets daily at 1:00 AM UTC\.
   + If you choose **Custom scan schedule**, select the **Scanning frequency**\.
   + If you choose **Daily**, enter the time, in UTC, that you want to scan your targets\. 
   + If you choose **Custom CRON Expression**, enter the schedule as a **CRON expression**\. For more information about formatting CRON expressions for Systems Manager, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

     Also, select **Wait to scan targets until first CRON interval**\. By default, Patch Manager immediately scans nodes as they become targets\.

1. If you chose **Scan and install**, choose the **Installation schedule** to use when installing patches to the specified targets\. If you choose **Use recommended defaults**, Patch Manager will install weekly patches at 2:00 AM UTC on Sunday\.
   + If you choose **Custom install schedule**, select the **Installation Frequency**\.
   + If you choose **Daily**, enter the time, in UTC, that you want to install updates on your targets\.
   + If you choose **Custom CRON expression**, enter the schedule as a **CRON expression**\. For more information about formatting CRON expressions for Systems Manager, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

     Also, clear **Wait to install updates until first CRON interval** to immediately install updates on nodes as they become targets\. By default, Patch Manager waits until the first CRON interval to install updates\.
   + Choose **Reboot if needed** to reboot the nodes after patch installation\. Rebooting after installation is recommended but can cause availability issues\.

1. In the **Patch baseline** section, choose the patch baselines to use when scanning and updating your targets\. 

   By default, Patch Manager uses the predefined patch baselines\. For more information, see [About predefined baselines](sysman-patch-baselines.md#patch-manager-baselines-pre-defined)\.

   If you choose **Custom patch baseline**, change the selected patch baseline for operating systems that you don't want to use a predefined AWS patch baseline\.

   The patch baselines available in Quick Setup, whether you use AWS predefined patch baselines or custom patch baselines, are those of the Home Region you selected\.
**Note**  
When creating a patch policy for your organization, Quick Setup creates an Amazon S3 bucket in your organization's management account that contains the baselines used by the patch policy\. 
**Important**  
If you are using a [patch policy configuration](patch-policies-about.md) in Quick Setup, updates you make to custom patch baselines are synchronized with Quick Setup once an hour\.   
If a custom patch baseline that was referenced in a patch policy is deleted, a banner displays on the Quick Setup **Configuration details** page for your patch policy\. The banner informs you that the patch policy references a patch baseline that no longer exists, and that subsequent patching operations will fail\. In this case, return to the Quick Setup **Configurations** page, select the Patch Manager configuration , and choose **Actions**, **Edit configuration**\. The deleted patch baseline name is highlighted, and you must select a new patch baseline for the affected operating system\.

1. \(Optional\) In the **Patching log storage** section, select **Write output to S3 bucket** to store patching operation logs in an Amazon S3 bucket\. 
**Note**  
If you are setting up a patch policy for an organization, the management account for your organization must have at least read\-only permissions for this bucket\. All organization units included in the policy must have write\-access to the bucket\. For information about granting bucket access to different accounts, see [Example 2: Bucket owner granting cross\-account bucket permissions](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-walkthroughs-managing-access-example2.html) in the *Amazon Simple Storage Service User Guide*\.

1. Provide the **S3 URI** or **Browse S3** for the bucket that you want to store patch log output in\. The management account must have read access to this bucket\. All non\-management accounts and targets configured in the **Targets** section must have write access to the provided S3 bucket for logging\.

1. In the **Targets** section, choose one of the following to identify the accounts and Regions for this patch policy operation\.
**Note**  
If you are working in a single account, options for working with organizations and organizational units \(OUs\) are not available\. You can choose whether to apply this configuration to all AWS Regions in your account or only the Regions you choose\.
   + **Entire organization** – All accounts and Regions in your organization\.
   + **Custom** – Only the OUs and Regions that you specify\.
     + In the **Target OUs** section, select the OUs where you want to set up the patch policy\. 
     + In the **Target Regions** section, select the Regions where you want to apply the patch policy\. 
   + **Current account** – Only the Regions you specify in the account you are currently signed into are targeted\. Choose one of the following:
     + **Current Region** – Only managed nodes in the Region selected in the console are targeted\. 
     + **Choose Regions** – Choose the individual Regions to apply the patch policy to\.

1. For **Choose how you want to target instances**, choose one of the following to identify the nodes to patch: 
   + **All managed nodes** – All managed nodes in the selected OUs and Regions\.
**Note**  
This option currently supports only Amazon EC2 instances\.
   + **Specify the resource group** – Choose the name of a resource group from the list to target its associated resources\.
**Note**  
Currently, selecting resource groups is supported only for single account configurations\. To patch resources in multiple accounts, choose a different targeting option\.
   + **Specify a node tag** – Only nodes tagged with the key\-value pair that you specify are patched in all accounts and Regions you have targeted\. 
   + **Manual** – Choose managed nodes from all specified accounts and Regions manually from a list\.

1. In the **Rate control** section, do the following:
   + For **Concurrency**, enter a number or percentage of nodes to run the patch policy on at the same time\.
   + For **Error threshold**, enter the number or percentage of nodes that can experience an error before the patch policy fails\.

1. \(Optional\) If you want, apply the IAM policies created by this Quick Setup configuration to nodes that already have an instance profile attached \(EC2 instances\) or a service role attached \(hybrid, non\-EC2 nodes\)\. Select **Add required IAM policies to existing instance profiles attached to your instances**\.

   Select this option when your managed nodes already have an instance profile or service role attached, but it doesn't contain all the permissions required for working with Systems Manager\.

   Your selection here is applied to managed nodes created later in the accounts and Regions that this patch policy configuration applies to\.

1. Choose **Create**\.

   To review patching status after the patch policy is created, you can access the configuration from the [https://console.aws.amazon.com/systems-manager/quick-setup](https://console.aws.amazon.com/systems-manager/quick-setup) page\.