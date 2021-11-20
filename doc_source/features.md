# Systems Manager capabilities<a name="features"></a>

Systems Manager groups capabilities into the following capability types:

**Topics**
+ [Quick Setup](#features-quick-setup)
+ [Operations Management](#features-operations-management)
+ [Application Management](#systems-manager-application-management)
+ [Change Management](#features-actions-and-change)
+ [Node Management](#features-instances-and-nodes)
+ [Shared Resources](#features-shared)

## Quick Setup<a name="features-quick-setup"></a>

Use [Quick Setup](systems-manager-quick-setup.md), a capability of AWS Systems Manager, to configure frequently used AWS services and features with recommended best practices\. You can use Quick Setup in an individual AWS account or across multiple AWS accounts and AWS Regions by integrating with AWS Organizations\. Quick Setup simplifies setting up services, including Systems Manager, by automating common or recommended tasks\. These tasks include, for example, creating required AWS Identity and Access Management \(IAM\) instance profile roles and setting up operational best practices, such as periodic patch scans and inventory collection\.

## Operations Management<a name="features-operations-management"></a>

Operations Management is a suite of capabilities that help you manage your AWS resources\. Choose the tabs to learn more\.

------
#### [ Incident Manager ]

[Incident Manager](incident-manager.md) is an incident management console that helps users mitigate and recover from incidents affecting their AWS hosted applications\.

Incident Manager increases incident resolution by notifying responders of impact, highlighting relevant troubleshooting data, and providing collaboration tools to get services back up and running\. Incident Manager also automates response plans and allows responder team escalation\. 

------
#### [ Explorer ]

[Explorer](Explorer.md), a capability of AWS Systems Manager, is a customizable operations dashboard that reports information about your AWS resources\. Explorer displays an aggregated view of operations data \(OpsData\) for your AWS accounts and across AWS Regions\. In Explorer, OpsData includes metadata about your Amazon EC2 instances, patch compliance details, and operational work items \(OpsItems\)\. Explorer provides context about how OpsItems are distributed across your business units or applications, how they trend over time, and how they vary by category\. You can group and filter information in Explorer to focus on items that are relevant to you and that require action\. When you identify high priority issues, you can use OpsCenter, a capability of AWS Systems Manager, to run Automation runbooks and resolve those issues\. 

------
#### [ OpsCenter ]

[OpsCenter](OpsCenter.md), a capability of AWS Systems Manager, provides a central location where operations engineers and IT professionals can view, investigate, and resolve operational work items \(OpsItems\) related to AWS resources\. OpsCenter is designed to reduce mean time to resolution for issues impacting AWS resources\. This Systems Manager capability aggregates and standardizes OpsItems across services while providing contextual investigation data about each OpsItem, related OpsItems, and related resources\. OpsCenter also provides Systems Manager Automation runbooks that you can use to resolve issues\. You can specify searchable, custom data for each OpsItem\. You can also view automatically generated summary reports about OpsItems by status and source\. 

------
#### [ CloudWatch Dashboards ]

[Amazon CloudWatch Dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) are customizable pages in the CloudWatch console that you can use to monitor your resources in a single view, even those resources that are spread across different regions\. You can use CloudWatch dashboards to create customized views of the metrics and alarms for your AWS resources\. 

------
#### [ Trusted Advisor & AWS Personal Health Dashboard \(PHD\) ]

Systems Manager hosts two online tools to help you provision your resources and monitor your account for health events\. Trusted Advisor is an online tool that provides you real time guidance to help you provision your resources following AWS best practices\. For more information, see [Trusted Advisor](http://aws.amazon.com/premiumsupport/technology/trusted-advisor/)\.

The AWS Personal Health Dashboard provides information about AWS Health events that can affect your account\. The information is presented in two ways: a dashboard that shows recent and upcoming events organized by category, and a full event log that shows all events from the past 90 days\. For more information, see [Getting Started with the AWS Personal Health Dashboard](https://docs.aws.amazon.com/health/latest/ug/getting-started-phd.html)\.

------

## Application Management<a name="systems-manager-application-management"></a>

Application Management is a suite of capabilities that help you manage your applications running in AWS\. Choose the tabs to learn more\.

------
#### [ Application Manager ]

[Application Manager](application-manager.md), a capability of AWS Systems Manager, helps you investigate and remediate issues with your AWS resources in the context of your applications\. Application Manager aggregates operations information from multiple AWS services and Systems Manager capabilities to a single AWS Management Console\.

------
#### [ Resource Groups ]

[AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html): An AWS *resource* is an entity you can work with in AWS, such as SSM documents, patch baselines, maintenance windows, parameters, and managed instances; an Amazon Elastic Compute Cloud \(Amazon EC2\) instance; an Amazon Elastic Block Store \(Amazon EBS\) volume; a security group; or an Amazon Virtual Private Cloud \(VPC\)\. A *resource group* is a collection of AWS resources that are all in the same AWS Region, and that match criteria provided in a query\. You build queries in the Resource Groups console, or pass them as arguments to AWS Resource Groups commands in the AWS CLI\. With Resource Groups, you can create a custom console that organizes and consolidates information based on criteria that you specify in tags\. You can also use groups as the basis for viewing monitoring and configuration insights in Systems Manager\.

------
#### [ AppConfig ]

[AppConfig](appconfig.md), a capability of AWS Systems Manager, helps you create, manage, and deploy application configurations\. AppConfig supports controlled deployments to applications of any size\. You can use AppConfig with applications hosted on Amazon EC2 instances, AWS Lambda containers, mobile applications, or IoT devices\. To prevent errors when deploying application configurations, AppConfig includes validators\. A validator provides a syntactic or semantic check to verify that the configuration you want to deploy works as intended\. During a configuration deployment, AppConfig monitors the application to verify that the deployment is successful\. If the system encounters an error or if the deployment invokes an alarm, AppConfig rolls back the change to minimize impact for your application users\.

------
#### [ Parameter Store ]

[Parameter Store](systems-manager-parameter-store.md), a capability of AWS Systems Manager, provides secure, hierarchical storage for configuration data and secrets management\. You can store data such as passwords, database strings, Amazon Elastic Compute Cloud \(Amazon EC2\) instance IDs and Amazon Machine Image \(AMI\) IDs, and license codes as parameter values\. You can store values as plain text or encrypted data\. You can then reference values by using the unique name you specified when you created the parameter\.

------

## Change Management<a name="features-actions-and-change"></a>

Systems Manager provides the following capabilities for taking action on or changing your AWS resources\. Choose the tabs to learn more\.

------
#### [ Change Manager ]

[Change Manager](change-manager.md), a capability of AWS Systems Manager, is an enterprise change management framework for requesting, approving, implementing, and reporting on operational changes to your application configuration and infrastructure\. From a single *delegated administrator account*, if you use AWS Organizations, you can manage changes across multiple AWS accounts in multiple AWS Regions\. Alternatively, using a *local account*, you can manage changes for a single AWS account\. Use Change Manager for managing changes to both AWS resources and on\-premises resources\.

------
#### [ Automation ]

Use [Automation](systems-manager-automation.md), a capability of AWS Systems Manager, to automate common maintenance and deployment tasks\. You can use Automation to create and update Amazon Machine Images \(AMIs\), apply driver and agent updates, reset passwords on Windows Server instance, reset SSH keys on Linux instances, and apply OS patches or application updates\. 

------
#### [ Change Calendar ]

[Change Calendar](systems-manager-change-calendar.md), a capability of AWS Systems Manager, helps you set up date and time ranges when actions you specify \(for example, in [Systems Manager Automation](systems-manager-automation.md) runbooks\) can or can't be performed in your AWS account\. In Change Calendar, these ranges are called *events*\. When you create a Change Calendar entry, you're creating a [Systems Manager document](sysman-ssm-docs.md) of the type `ChangeCalendar`\. In Change Calendar, the document stores [iCalendar 2\.0](https://icalendar.org/) data in plaintext format\. Events that you add to the Change Calendar entry become part of the document\. You can add events manually in the Change Calendar interface or import events from a supported third\-party calendar using an `.ics` file\.

------
#### [ Maintenance Windows ]

Use [Maintenance Windows](systems-manager-maintenance.md), a capability of AWS Systems Manager, to set up recurring schedules for managed instances to run administrative tasks such as installing patches and updates without interrupting business\-critical operations\. 

------

## Node Management<a name="features-instances-and-nodes"></a>

Systems Manager provides the following capabilities for managing your Amazon EC2 instances, your on\-premises servers and virtual machines \(VMs\) in your hybrid environment, and other types of AWS resources \(nodes\)\. Choose the tabs to learn more\.

------
#### [ Compliance ]

Use [Compliance](systems-manager-compliance.md), a capability of AWS Systems Manager, to scan your fleet of managed instances for patch compliance and configuration inconsistencies\. You can collect and aggregate data from multiple AWS accounts and AWS Regions, and then drill down into specific resources that aren’t compliant\. By default, Compliance displays compliance data about Patch Manager patching and State Manager associations\. You can also customize the service and create your own compliance types based on your IT or business requirements\.

------
#### [ Fleet Manager ]

[Fleet Manager](fleet.md), a capability of AWS Systems Manager, is a unified user interface \(UI\) experience that helps you remotely manage your server fleet running on AWS, or on\-premises\. With Fleet Manager, you can view the health and performance status of your entire server fleet from one console\. You can also gather data from individual instances to perform common troubleshooting and management tasks from the console\. This includes viewing directory and file contents, Windows registry management, operating system user management, and more\. 

------
#### [ Inventory ]

[Inventory](systems-manager-inventory.md), a capability of AWS Systems Manager, automates the process of collecting software inventory from managed instances\. You can use Inventory to gather metadata about applications, files, components, patches, and more on your managed instances\.

------
#### [ Session Manager ]

Use [Session Manager](session-manager.md), a capability of AWS Systems Manager, to manage your Amazon Elastic Compute Cloud \(Amazon EC2\) instances through an interactive one\-click browser\-based shell or through the AWS CLI\. Session Manager provides secure and auditable instance management without needing to open inbound ports, maintain bastion hosts, or manage SSH keys\. Session Manager also allows you to comply with corporate policies that require controlled access to instances, strict security practices, and fully auditable logs with instance access details, while still providing end users with simple one\-click cross\-platform access to your EC2 instances\. 

------
#### [ Run Command ]

Use [Run Command](execute-remote-commands.md), a capability of AWS Systems Manager, to remotely and securely manage the configuration of your managed instances at scale\. Use Run Command to perform on\-demand changes such as updating applications or running Linux shell scripts and Windows PowerShell commands on a target set of dozens or hundreds of instances\. 

------
#### [ State Manager ]

Use [State Manager](systems-manager-state.md), a capability of AWS Systems Manager, to automate the process of keeping your managed instances in a defined state\. You can use State Manager to guarantee that your instances are bootstrapped with specific software at startup, joined to a Windows domain \(Windows Server instances only\), or patched with specific software updates\. 

------
#### [ Patch Manager ]

Use [Patch Manager](systems-manager-patch.md) to automate the process of patching your managed instances with both security related and other types of updates\. You can use Patch Manager to apply patches for both operating systems and applications\. \(On Windows Server, application support is limited to updates for applications released by Microsoft\.\) This capability allows you to scan instances for missing patches and apply missing patches individually or to large groups of instances by using EC2 instance tags\. Patch Manager uses *patch baselines*, which can include rules for auto\-approving patches within days of their release, and a list of approved and rejected patches\. You can install security patches on a regular basis by scheduling patching to run as a Systems Manager maintenance window task, or you can patch your managed instances on demand at any time\. For Linux operating systems, you can define the repositories that should be used for patching operations as part of your patch baseline\. This allows you to ensure that updates are installed only from trusted repositories regardless of what repositories are configured on the instance\. For Linux, you also have the ability to update any package on the instance, not just those that are classified as operating system security updates\. You can also generate patch reports that are sent to an S3 bucket of your choice\. For a single instance, reports include details of all patches for the instance\. For a report on all instances, only a summary of how many patches are missing is provided\.

------
#### [ Distributor ]

Use [Distributor](distributor.md), a capability of AWS Systems Manager, to create and deploy packages to managed instances\. With Distributor, you can package your own software—or find AWS\-provided agent software packages, such as **AmazonCloudWatchAgent**—to install on Systems Manager managed instances\. After you install a package for the first time, you can use Distributor to uninstall and reinstall a new package version, or perform an in\-place update that adds new or changed files\. Distributor publishes resources, such as software packages, to Systems Manager managed instances\.

------
#### [ Hybrid Activations ]

To set up servers and VMs in your hybrid environment as managed instances, create a managed instance [activation](systems-manager-managedinstances.md)\. After you complete the activation, you receive an activation code and ID\. This code and ID combination functions such as an Amazon Elastic Compute Cloud \(Amazon EC2\) access ID and secret key to provide secure access to the Systems Manager service from your managed instances\.

------

## Shared Resources<a name="features-shared"></a>

Systems Manager uses the following shared resources for managing and configuring your AWS resources\. Choose the tabs to learn more\.

------
#### [ Documents ]

A [Systems Manager document](sysman-ssm-docs.md) \(SSM document\) defines the actions that Systems Manager performs\. SSM document types include *Command* documents, which are used by State Manager and Run Command, and Automation runbooks, which are used by Systems Manager Automation\. Systems Manager includes dozens of pre\-configured documents that you can use by specifying parameters at runtime\. Documents can be expressed in JSON or YAML, and include steps and parameters that you specify\.

------