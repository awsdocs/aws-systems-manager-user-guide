# AWS Systems Manager OpsCenter<a name="OpsCenter"></a>

OpsCenter, a capability of AWS Systems Manager, provides a central location where operations engineers and IT professionals can manage operational work items \(OpsItems\) related to AWS resources\. An *OpsItem* is any operational issue or interruption that needs investigation and remediation\. Using OpsCenter, you can view contextual investigation data about each OpsItem, including related OpsItems and related resources\. You can also run Systems Manager Automation runbooks to resolve OpsItems\. 

 Each OpsItem includes the relevant information, such as name and ID of the AWS resource that generated the OpsItem, that is required to resolve an event\. When you set up OpsCenter and integrate it with other AWS services, it can create OpsItems automatically\. If integrated with these services, OpsCenter displays information from AWS Config, AWS CloudTrail, and Amazon EventBridge to help you investigate an OpsItem\. As a result, you don't have to navigate between console pages for your investigation\. 

You can use OpsCenter to investigate and remediate issues with your on\-premises managed nodes that are configured for Systems Manager\. For more information about setting up and configuring on\-premises servers and virtual machines for Systems Manager, see [Setting up Systems Manager for hybrid environments](systems-manager-managedinstances.md)\.

You can work with OpsCenter by using the Systems Manager console, AWS Command Line Interface \(AWS CLI\), AWS Tools for PowerShell, or the AWS SDK of your choice\. Using AWS Identity and Access Management \(IAM\) policies, you can decide which members of your organization can create, view, list, and update OpsItems\. You can assign tags to OpsItems and then create IAM policies that give access to users and groups based on tags\. 

**Note**  
 There is a charge to use OpsCenter\. For information, see [AWS Systems Manager Pricing](http://aws.amazon.com/systems-manager/pricing/)\.  
You can view quotas for all Systems Manager capabilities in [Systems Manager service quotas](https://docs.aws.amazon.com/general/latest/gr/ssm.html#limits_ssm) in the *Amazon Web Services General Reference*\. Unless otherwise noted, each quota is Region\-specific\.

## OpsCenter workflow<a name="OpsCenter-process"></a>

To set up and work with OpsCenter to remediate OpsItems, perform the following steps:

1. [Set up OpsCenter using Integrated Setup](OpsCenter-setup.md)\. You can also [set up OpsCenter to centrally manage OpsItems across accounts ](OpsCenter-getting-started-multiple-accounts.md) if needed\. 

1. [Integrate AWS services with OpsCenter](OpsCenter-applications-that-integrate.md)\. OpsCenter can integrate with Amazon CloudWatch, Amazon CloudWatch Application Insights, Amazon EventBridge, Amazon DevOps Guru, AWS Config, AWS Security Hub, and AWS Systems Manager Incident Manager\. 

1. [Create OpsItems](OpsCenter-create-OpsItems.md)\. You can create OpsItems automatically and manually\.  

1. \(Optional\) [Set up Amazon Simple Notification Service to receive notifications](OpsCenter-getting-started-sns.md)\. 

1.  [Manage OpsItems](OpsCenter-working-with-OpsItems.md) by adding context on related resources, related OpsItems, and operational data, and removing duplicate OpsItems\. 

1. [Remediate OpsItems](OpsCenter-remediating.md) using Systems Manager Automation runbooks\. 