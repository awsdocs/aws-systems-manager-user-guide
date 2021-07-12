# AWS Systems Manager Quick Setup<a name="systems-manager-quick-setup"></a>

Use Quick Setup, a capability of AWS Systems Manager, to quickly configure frequently used Amazon Web Services services and features with recommended best practices\. You can use Quick Setup in an individual account or across multiple AWS accounts and AWS Regions by integrating with AWS Organizations\. Quick Setup simplifies setting up services, including Systems Manager, by automating common or recommended tasks\. These tasks include, for example, creating required AWS Identity and Access Management \(IAM\) instance profile roles and setting up operational best practices, such as periodic patch scans and inventory collection\. 

Quick Setup is most beneficial for customers who already have some experience with the services and features they're setting up, and want to simplify their setup process\. If you are unfamiliar with the Amazon Web Services service you're configuring with Quick Setup, we recommend learning more about the service by reviewing the content in the relevant User Guide before creating a configuration with Quick Setup\.

Once configured, the Quick Setup dashboard displays a real\-time view of your configuration deployment status\. This dashboard also helps you to track the setup of services across accounts and Regions\. Furthermore, Quick Setup periodically checks for and helps you mitigate drift from your initial configuration\. 

To access this capability, choose **Quick Setup** in the navigation pane of the Systems Manager console\. To access the **Organization Quick Setup** type, which you can use to target multiple accounts and Regions, you must be logged into the management account for your organization\. For more information, see [Getting started with AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_getting-started.html) in the *AWS Organizations User Guide*\. 

To get started with Quick Setup choose a service or feature in the list of available configuration types\. A *configuration type* in Quick Setup is specific to an AWS service or feature\. When you choose a configuration type, you choose the options that you want to configure for that service or feature\. By default, configuration types help you set up the service or feature to use recommended best practices\. 

For more information about the Host Management configuration type, see [Quick Setup Host Management](quick-setup-host-management.md)\. 

For more information about the Change Manager configuration type, see [Setting up Change Manager for an organization \(management account\)](change-manager-organization-setup.md)\.

**Note**  
The deployment configuration process for Change Manager, a capability of AWS Systems Manager, can't be performed in the following AWS Regions:  
Europe \(Milan\) \(eu\-south\-1\)
Middle East \(Bahrain\) \(me\-south\-1\)
Africa \(Cape Town\) \(af\-south\-1\)
Asia Pacific \(Hong Kong\) \(ap\-east\-1\)
Ensure that you're working in a different Region in your management account for [this procedure](change-manager-organization-setup.md)\.

After setting up a configuration, you can view details about it and its deployment status across organizational units \(OUs\) and Regions\. You can also view State Manager association status for the configuration\. State Manager is a capability of AWS Systems Manager\. In the **Configuration details** pane, you can view a summary of the Quick Setup configuration\. This summary includes details from all accounts and any detected configuration drift\. 

*Configuration drift* occurs whenever a user makes any change to a service or feature that conflicts with the selections made through Quick Setup\. Quick Setup periodically checks for configuration drift and attempts to remediate it\. This helps ensure that your service or feature maintains the desired configuration state\. 

**Topics**
+ [Quick Setup Host Management](quick-setup-host-management.md)
+ [AWS Config recording](quick-setup-config.md)
+ [Configure DevOps Guru with Quick Setup](quick-setup-devops.md)
+ [Deploy Distributor packages with Quick Setup](quick-setup-distributor.md)