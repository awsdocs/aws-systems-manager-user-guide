# Setting up a delegated administrator for Systems Manager<a name="setting_up_delegated_admin"></a>

When you set up an organization in AWS Organizations, you assign a management account to perform all administrative tasks for all AWS services\. The management account user can assign a *delegated administrator account* only for Systems Manager to perform administrative tasks for Change Manager, Explorer, and OpsCenter\. AWS Organizations is an account management service that you can use to create an organization and assign AWS accounts to manage these accounts centrally\. For information about AWS Organizations, see [AWS Organizations](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_introduction.html) in the *AWS Organizations User Guide*\. 

 Change Manager, Explorer, and OpsCenter, capabilities of AWS Systems Manager, work with AWS Organizations to perform tasks on all member accounts of your organization\. You can assign only one delegated administrator for all Systems Manager capabilities\. The delegated administrator account must be the member of the organizational to which it's assigned\. 

**Topics**
+ [Delegated administrator for Change Manager](#setting_up_delegated_administrator_change_manager)
+ [Delegated administrator for Explorer](#setting_up_delegated_administrator_explorer)
+ [Delegated administrator for OpsCenter](#setting_up_delegated_administrator_opscenter)

## Delegated administrator for Change Manager<a name="setting_up_delegated_administrator_change_manager"></a>

 Change Manager is an enterprise change management framework for requesting, approving, implementing, and reporting on operational changes to your application configuration and infrastructure\. 

If you use Change Manager across an organization, assign a delegated administrator account to manage change templates, approvals, and reporting for all member accounts\. Using Quick Setup, you can set up Change Manager to use with an organization and select the delegated administrator account\. If you use Change Manager with a single AWS account, the delegated administrator account isn't required\. 

By default, Change Manager displays all change\-related tasks in the delegated administrator account\. For instructions on configuring a delegated administrator while setting up Change Manager for an organization, see [Setting up Change Manager for an organization \(management account\)](change-manager-organization-setup.md)\.

**Important**  
If you use Change Manager across an organization, we recommend always making changes from the delegated administrator account\. Although you can make changes from other accounts in the organization, those changes won't be reported in or viewable from the delegated administrator account\.

## Delegated administrator for Explorer<a name="setting_up_delegated_administrator_explorer"></a>

Explorer is a customizable operations dashboard that reports aggregated view of operations data \(OpsData\) for your AWS accounts, across AWS Regions\.

 You can configure a delegated administrator account for Systems Manager to aggregate Explorer data from multiple Regions and accounts by using resource data sync with AWS Organizations\. A delegated administrator can search, filter, and aggregate Explorer data using the AWS Management Console, the AWS Command Line Interface \(AWS CLI\), or AWS Tools for Windows PowerShell\. 

When you use a delegated administrator account for Explorer, you limit the number of administrators who can create or delete multi\-account and Region resource data syncs to an individual AWS account\. 

You can synchronize operations data across all AWS accounts in your organization by using Explorer\. For information on how to assign a delegated administrator from Explorer, see [Configuring a delegated administrator](Explorer-setup-delegated-administrator.md)\. 

## Delegated administrator for OpsCenter<a name="setting_up_delegated_administrator_opscenter"></a>

OpsCenter provides a central location where operations engineers and IT professionals can manage operational work items \(OpsItems\) related to AWS resources\. If you want to use OpsCenter to manage OpsItems centrally across accounts, you must set up the organization in AWS Organizations\. 



The delegated administrator can create OpsItems for all selected member accounts and manage those OpsItems\. You can set up the delegated administrator in Explorer\. For information on how to assign a delegated administrator, see [Configuring a delegated administrator](Explorer-setup-delegated-administrator.md)\.