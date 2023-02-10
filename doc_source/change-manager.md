# AWS Systems Manager Change Manager<a name="change-manager"></a>

Change Manager, a capability of AWS Systems Manager, is an enterprise change management framework for requesting, approving, implementing, and reporting on operational changes to your application configuration and infrastructure\. From a single *delegated administrator account*, if you use AWS Organizations, you can manage changes across multiple AWS accounts and across AWS Regions\. Alternatively, using a *local account*, you can manage changes for a single AWS account\. Use Change Manager for managing changes to both AWS resources and on\-premises resources\. To get started with Change Manager, open the [Systems Manager console](https://console.aws.amazon.com/systems-manager/change-manager)\. In the navigation pane, choose **Change Manager**\.

With Change Manager, you can use pre\-approved *change templates* to help automate change processes for your resources and help avoid unintentional results when making operational changes\. Each change template specifies the following:
+ One or more Automation runbooks for a user to choose from when creating a change request\. The changes that are made to your resources are defined in Automation runbooks\. You can include custom runbooks or [AWS managed runbooks](automation-documents-reference.md) in the change templates you create\. When a user creates a change request, they can choose which one of the available runbooks to include in the request\. Additionally, you can create change templates that let the user making the request specify any runbook in the change request\.
+ The users in the account who must review change requests that were made using that change template\.
+ The Amazon Simple Notification Service \(Amazon SNS\) topic that is used to notify assigned approvers that a change request is ready for review\.
+ The Amazon CloudWatch alarm that is used to monitor the runbook workflow\.
+ The Amazon SNS topic that is used to send notifications about status changes for change requests that are created using the change template\.
+ The tags to apply to the change template for use in categorizing and filtering your change templates\.
+ Whether change requests created from the change template can be run without an approval step \(auto\-approved requests\)\.

Through its integration with Change Calendar, which is another capability of Systems Manager, Change Manager also helps you safely implement changes while avoiding schedule conflicts with important business events\. Change Manager integration with AWS Organizations and AWS IAM Identity Center \(successor to AWS Single Sign\-On\) helps you manage changes across your organization from a single account using your existing identity management system\. You can monitor change progress from Change Manager and audit operational changes across your organization, providing improved visibility and accountability\.

Change Manager complements the safety controls of your [continuous integration](http://aws.amazon.com/devops/continuous-integration) \(CI\) practices and [continuous delivery](http://aws.amazon.com/devops/continuous-delivery) \(CD\) methodology\. Change Manager isn't intended for changes made as part of an automated release process, such as a CI/CD pipeline, unless there is an exception or approval required\.

## How Change Manager works<a name="how-change-manager-works"></a>

When the need for a standard or emergency operational change is identified, someone in the organization creates a change request that is based on one of the change templates created for use in your organization or account\.

If the requested change requires manual approvals, Change Manager notifies the designated approvers through an Amazon SNS notification that a change request is ready for their review\. You can designate approvers for change requests in the change template, or let users designate approvers the change request itself\. You can assign different reviewers to different templates\. For example, assign one user, user group, or AWS Identity and Access Management \(IAM\) role who must approve requests for changes to managed nodes, and another user, group, or IAM role for database changes\. If the change template allows auto\-approvals, and a requester's user policy doesn't prohibit it, the user can also choose to run the Automation runbook for their request without a review step \(with the exception of change freeze events\)\.

For each change template, you can add up to five levels of approvers\. For example, you might require technical reviewers to approve a change request created from a change template first, and then require a second level of approvals from one or more managers\.

Change Manager is integrated with [AWS Systems Manager Change Calendar](systems-manager-change-calendar.md)\. When a requested change is approved, the system first determines whether the request conflicts with other scheduled business activities\. If a conflict is detected, Change Manager can block the change or require additional approvals before starting the runbook workflow\. For example, you might allow changes only during business hours to ensure that teams are available to manage any unexpected problems\. For any changes requested to run outside those hours, you can require higher\-level management approval in the form of *change freeze approvers*\. For emergency changes, Change Manager can skip the step of checking Change Calendar for conflicts or blocking events after a change request is approved\.

When it's time to implement an approved change, Change Manager runs the Automation runbook that is specified in the associated change request\. Only the operations defined in approved change requests are permitted when runbook workflows run\. This approach helps you avoid unintentional results while changes are being implemented\.

In addition to restricting the changes that can be made when a runbook workflow runs, Change Manager also helps you control concurrency and error thresholds\. You choose how many resources a runbook workflow can run on at once, how many accounts the change can run in at once, and how many failures to allow before the process is stopped and \(if the runbook includes a rollback script\) rolled back\. You can also monitor the progress of changes being made by using CloudWatch alarms\.

After a runbook workflow has completed, you can review details about the changes made\. These details include the reason for a change request, which change template was used, who requested and approved the changes, and how the changes were implemented\.

**More info**  
[Introducing AWS Systems ManagerChange Manager](http://aws.amazon.com/blogs/aws/introducing-systems-manager-change-manager/) on the *AWS News Blog*

## How can Change Manager benefit my operations?<a name="change-manager-benefits"></a>

Benefits of Change Manager include the following:
+ **Reduce risk of service disruption and downtime**

  Change Manager can make operational changes safer by ensuring that only approved changes are implemented when a runbook workflow runs\. You can block unplanned and unreviewed changes\. Change Manager helps you avoid the types of unintentional results caused by human error that require costly hours of research and backtracking\.
+ **Get detailed auditing and reporting on change histories**

  Change Manager provides accountability with a consistent way to report and audit changes made across your organization, the intent of the changes, and details about who approved and implemented them\.
+ **Avoid schedule conflicts or violations**

  Change Manager can detect schedule conflicts such as holiday events or new product launches, based on the active change calendar for your organization\. You can allow runbook workflows to run only during business hours, or allow them only with additional approvals\.
+ **Adapt change requirements to your changing business**

  During different business periods, you can implement different change management requirements\. For example, during end\-of\-month reporting, tax season, or other critical business periods, you can block changes or require director\-level approval for changes that could introduce unnecessary operational risks\.
+ **Centrally manage changes across accounts**

  Through its integration with Organizations, Change Manager makes it possible for you to manage changes throughout all of your organizational units \(OUs\) from a single delegated administrator account\. You can turn on Change Manager for use with your entire organization or with only some of your OUs\.

## Who should use Change Manager?<a name="change-manager-who"></a>

Change Manager is appropriate for the following AWS customers and organizations:
+ Any AWS customer who wants to improve the safety and governance of operational changes made to their cloud or on\-premises environments\.
+ Organizations that want to increase collaboration and visibility across teams, improve application availability by avoiding downtime, and reduce the risk associated with manual and repetitive tasks\.
+ Organizations that must comply with best practices for change management\. 
+ Customers who need a fully auditable history of changes made to their application configuration and infrastructure\.

## What are the main features of Change Manager?<a name="change-manager-features"></a>

Primary features of Change Manager include the following:
+ **Integrated support for change management best practices**

  With Change Manager, you can apply select change management best practices to your operations\. You can choose to turn on the following options:
  + Check Change Calendar to see if events are currently restricted so changes are made only during open calendar periods\.
  + Allow changes during restricted events with extra approvals from change freeze approvers\.
  + Require CloudWatch alarms to be specified for all change templates\.
  + Require all change templates created in your account to be reviewed and approved before they can be used to create change requests\.
+ **Different approval paths for closed calendar periods and emergency change requests**

  You can allow an option to check Change Calendar for restricted events and block approved change requests until the event is complete\. However, you can also designate a second group of approvers, change freeze approvers, who can permit the change to be made even if the calendar is closed\. You can also create emergency change templates\. Change requests created from an emergency change template still require regular approvals but aren't subject to calendar restrictions and don't require change freeze approvals\.
+ **Control how and when runbook workflows are started**

  Runbook workflows can be started according to a schedule, or as soon as approvals are complete \(subject to calendar restriction rules\)\.
+ **Built\-in notification support**

  Specify who in your organization should review and approve change templates and change requests\.  Assign an Amazon SNS topic to a change template to send notifications to the topic's subscribers about status changes for change requests created with that change template\.
+ **Integration with AWS Systems Manager Change Calendar**

  Change Manager allows administrators to restrict scheduling changes during specified time periods\. For instance, you can create a policy that allows changes only during business hours to ensure that the team is available to handle any issues\. You can also restrict changes during important business events\. For example, retail businesses might restrict changes during large sales events\. You can also require additional approvals during restricted periods\. 
+ **Integration with AWS IAM Identity Center \(successor to AWS Single Sign\-On\) and Active Directory support**

  With IAM Identity Center integration, members of your organization can access AWS accounts and manage their resources using Systems Manager based on a common user identity\. Using IAM Identity Center, you can assign your users access to accounts across AWS\.

  Integration with Active Directory makes it possible to assign users in your Active Directory account as approvers for change templates created for your Change Manager operations\.
+ **Integration with Amazon CloudWatch alarms**

  Change Manager is integrated with CloudWatch alarms\. Change Manager listens for CloudWatch alarms during the runbook workflow and takes any actions, including sending notifications, that are defined for the alarm\.
+ **Integration with AWS CloudTrail Lake**

  By creating an event data store in AWS CloudTrail Lake, you can view auditable information about the changes made by change requests that run in your account or organization\. The event information stored includes such details as the following:
  + The API actions that were run
  + Tthe request parameters included for those actions
  + The user that ran the action
  + The resources that were updated during the process
+ **Integration with AWS Organizations**

  Using the cross\-account capabilities provided by Organizations, you can use a delegated administrator account for managing Change Manager operations in OUs in your organization\. In your Organizations management account, you can specify which account is to be the delegated administrator account\. You can also control which of your OUs Change Manager can be used in\.

## Is there a charge to use Change Manager?<a name="change-manager-cost"></a>

Yes\. Change Manager is priced on a pay\-per\-use basis\. You pay only for what you use\. For more information, see [AWS Systems Manager Pricing](http://aws.amazon.com/systems-manager/pricing/)\.

## What are the primary components of Change Manager?<a name="change-manager-primary-components"></a>

Change Manager components that you use to manage the change process in your organization or account include the following:

### Delegated administrator account<a name="change-manager-what-is-delegated-account"></a>

If you use Change Manager across an organization, you use a delegated administrator account\. This is the AWS account designated as the account for managing operations activities across Systems Manager, including Change Manager\. The delegated administrator account manages change activities across your organization\. When you set up your organization for use with Change Manager, you specify which of your accounts serves in this role\. The delegated administrator account must be the only member of the organizational unit \(OU\) to which it's assigned\. The delegated administrator account isn't required if you use Change Manager with a single AWS account only\.

**Important**  
If you use Change Manager across an organization, we recommend always making changes from the delegated administrator account\. While you can make changes from other accounts in the organization, those changes won't be reported in or viewable from the delegated administrator account\.

### Change template<a name="change-manager-what-is-change-template"></a>

A change template is a collection of configuration settings in Change Manager that define such things as required approvals, available runbooks, and notification options for change requests\.

You can require that the change templates created by users in your organization or account go through an approval process before they can be used\.

Change Manager supports two types of change templates\. For an approved change request that is based on an *emergency change template*, the requested change can be made even if there are blocking events in Change Calendar\. For an approved change request that is based on a *standard change template*, the requested change can't be made if there are blocking events in Change Calendar unless additional approvals are received from designated *change freeze event * approvers\.

### Change request<a name="change-manager-what-is-change-request"></a>

A change request is a request in Change Manager to run an Automation runbook that updates one or more resources in your AWS or on\-premises environments\. A change request is created using a change template\.

When you create a change request, one or more approvers in your organization or account must review and approve the request\. Without the required approvals, the runbook workflow, which applies the changes you request, isn't permitted to run\.

In the system, change requests are a type of OpsItem in AWS Systems Manager OpsCenter\. However, OpsItems of the type `/aws/changerequest` aren't displayed in OpsCenter\. As OpsItems, change requests are subject to the same enforced quotas as other types of OpsItems\. For information about the number of OpsItems that can be created for an AWS account in an AWS Region, see [What are the quotas for OpsCenter?](OpsCenter.md#OpsCenter-learn-more-limits)\.

Additionally, to create a change request programmatically, you don't call the `CreateOpsItem` API operation\. Instead, you use the `[StartChangeRequestExecution](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_StartChangeRequestExecution.html)` API operation\. But rather than running immediately, the change request must be approved, and there must not any blocking events in Change Calendar to prevent the workflow from running\. When approvals have been received and the calendar isn't blocked \(or permission has been given to bypass blocking calendar events\), the `StartChangeRequestExecution` action is able to complete\.

### Runbook workflow<a name="change-manager-what-is-runbook-workflow"></a>

A runbook workflow is the process of requested changes being made to the targeted resources in your cloud or on\-premises environment\. Each change request designates a single Automation runbook to use to make the requested change\. The runbook workflow occurs after all required approvals have been granted and there are no blocking events in Change Calendar\. If the change has been scheduled for a specific date and time, the runbook workflow doesn't begin until scheduled, even if all approvals have been received and the calendar isn't blocked\.

### <a name="change-manager-what-is-OpsItem"></a>

**Topics**
+ [How Change Manager works](#how-change-manager-works)
+ [How can Change Manager benefit my operations?](#change-manager-benefits)
+ [Who should use Change Manager?](#change-manager-who)
+ [What are the main features of Change Manager?](#change-manager-features)
+ [Is there a charge to use Change Manager?](#change-manager-cost)
+ [What are the primary components of Change Manager?](#change-manager-primary-components)
+ [Setting up Change Manager](change-manager-setting-up.md)
+ [Working with Change Manager](working-with-change-manager.md)
+ [Auditing and logging Change Manager activity](change-manager-auditing.md)
+ [Troubleshooting Change Manager](change-manager-troubleshooting.md)