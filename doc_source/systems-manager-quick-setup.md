# AWS Systems Manager Quick Setup<a name="systems-manager-quick-setup"></a>

Use AWS Systems Manager Quick Setup to quickly configure frequently used AWS services and features with recommended best practices\. You can use Quick Setup in an individual account or across multiple accounts and AWS Regions by integrating with AWS Organizations\. Quick Setup simplifies setting up services, including AWS Systems Manager, by automating common or recommended tasks\. These tasks include, for example, creating required AWS Identity and Access Management \(IAM\) instance profile roles and setting up operational best practices, such as periodic patch scans and inventory collection\. 

Once configured, the Quick Setup dashboard displays a real\-time view of your configuration deployment status\. This dashboard also enables you to track the setup of services across accounts and Regions\. Furthermore, Quick Setup periodically checks for and helps you mitigate drift from your initial configuration\. 

To access this capability, choose **Quick Setup** in the navigation pane of the Systems Manager console\. To access the **Organization Quick Setup** type, which enables you to target multiple accounts and Regions, you must be logged into the management account for your organization\. For more information, see [Getting started with AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_getting-started.html) in the *AWS Organizations User Guide*\. 

To get started with Quick Setup choose a service or feature in the list of available configuration types\. A *configuration type* in Quick Setup is specific to an AWS service or feature\. When you choose a configuration type, you choose the options that you want to configure for that service or feature\. By default, configuration types help you set up the service or feature to use recommended best practices\. 

For more information about the Host Management configuration type, see [Quick Setup Host Management](quick-setup-host-management.md)\. 

For more information about the Change Manager configuration type, see [Setting up Change Manager for an organization \(management account\)](change-manager-organization-setup.md)\.

**Note**  
Currently, the deployment configuration process for Change Manager cannot be performed in the following AWS Regions:  
Europe \(Milan\) \(eu\-south\-1\)
Middle East \(Bahrain\) \(me\-south\-1\)
Africa \(Cape Town\) \(af\-south\-1\)
Asia Pacific \(Hong Kong\) \(ap\-east\-1\)
Ensure that you are working in a different Region in your management account for [this procedure](change-manager-organization-setup.md)\.

After setting up a configuration, you can view details about it and its deployment status across organizational units \(OUs\) and Regions\. You can also view Systems Manager State Manager association status for the configuration\. In the **Configuration details** pane, you can view a summary of the Quick Setup configuration\. This summary includes details from all accounts and any detected configuration drift\. 

*Configuration drift* occurs whenever a user makes any change to a service or feature that conflicts with the selections made through Quick Setup\. Quick Setup periodically checks for configuration drift and attempts to remediate it\. This helps ensure that your service or feature maintains the desired configuration state\. 

**Topics**
+ [Quick Setup Host Management](quick-setup-host-management.md)