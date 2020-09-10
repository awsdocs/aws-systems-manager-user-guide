# Systems Manager capabilities<a name="features"></a>

Systems Manager capabilities are grouped into the following capability types:

**Topics**
+ [Quick Setup](#features-quick-setup)
+ [Operations Management](#features-operations-management)
+ [Application Management](#systems-manager-application-management)
+ [Actions & Change](#features-actions-and-change)
+ [Instances & Nodes](#features-instances-and-nodes)
+ [Shared Resources](#features-shared)

## Quick Setup<a name="features-quick-setup"></a>

[Quick Setup](systems-manager-quick-setup.md) is a tool you can use to quickly configure required security roles and commonly used Systems Manager capabilities on your EC2 instances\. These capabilities help you manage and monitor the health of your instances while providing the minimum required permissions to get started\. Specifically, Quick Setup helps you configure the following components on the instances you choose or target by using tags:
+ AWS Identity and Access Management \(IAM\) instance profile roles for Systems Manager\.
+ A scheduled, bi\-weekly update of SSM Agent\.
+ A scheduled collection of Inventory metadata every 30 minutes\.
+ A daily scan of your instances to identify missing patches\.
+ A one\-time installation and configuration of the Amazon CloudWatch agent\.
+ A scheduled, monthly update of the CloudWatch agent\.

## Operations Management<a name="features-operations-management"></a>

Operations Management is a suite of capabilities that help you manage your AWS resources\. Choose the tabs to learn more\.

------
#### [ Explorer ]

[Explorer](Explorer.md) is a customizable operations dashboard that reports information about your AWS resources\. Explorer displays an aggregated view of operations data \(OpsData\) for your AWS accounts and across Regions\. In Explorer, OpsData includes metadata about your EC2 instances, patch compliance details, and operational work items \(OpsItems\)\. Explorer provides context about how OpsItems are distributed across your business units or applications, how they trend over time, and how they vary by category\. You can group and filter information in Explorer to focus on items that are relevant to you and that require action\. When you identify high priority issues, you can use Systems Manager OpsCenter to run Automation runbooks and quickly resolve those issues\. 

------
#### [ OpsCenter ]

[OpsCenter](OpsCenter.md) provides a central location where operations engineers and IT professionals can view, investigate, and resolve operational work items \(OpsItems\) related to AWS resources\. OpsCenter is designed to reduce mean time to resolution for issues impacting AWS resources\. This Systems Manager capability aggregates and standardizes OpsItems across services while providing contextual investigation data about each OpsItem, related OpsItems, and related resources\. OpsCenter also provides Systems Manager Automation documents \(runbooks\) that you can use to quickly resolve issues\. You can specify searchable, custom data for each OpsItem\. You can also view automatically\-generated summary reports about OpsItems by status and source\. 

------
#### [ CloudWatch Dashboards ]

[Amazon CloudWatch Dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) are customizable home pages in the CloudWatch console that you can use to monitor your resources in a single view, even those resources that are spread across different regions\. You can use CloudWatch dashboards to create customized views of the metrics and alarms for your AWS resources\. 

------
#### [ Trusted Advisor & Personal Health Dashboard \(PHD\) ]

Systems Manager hosts two online tools to help you provision your resources and monitor your account for health events\. Trusted Advisor is an online tool that provides you real time guidance to help you provision your resources following AWS best practices\. For more information, see [Trusted Advisor](https://aws.amazon.com/premiumsupport/technology/trusted-advisor/)\.

The AWS Personal Health Dashboard provides information about AWS Health events that can affect your account\. The information is presented in two ways: a dashboard that shows recent and upcoming events organized by category, and a full event log that shows all events from the past 90 days\. For more information, see [Getting Started with the AWS Personal Health Dashboard](https://docs.aws.amazon.com/health/latest/ug/getting-started-phd.html)\.

------

## Application Management<a name="systems-manager-application-management"></a>

Application Management is a suite of capabilities that help you manage your applications running in AWS\. Choose the tabs to learn more\.

------
#### [ Resource Groups ]

[AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html): An AWS *resource* is an entity you can work with in AWS, such as Systems Manager SSM documents, patch baselines, maintenance windows, parameters, and managed instances; an Amazon Elastic Compute Cloud \(EC2\) instance; an Amazon Elastic Block Store \(Amazon EBS\) volume; a security group; or an Amazon Virtual Private Cloud \(VPC\)\. A *resource group* is a collection of AWS resources that are all in the same AWS Region, and that match criteria provided in a query\. You build queries in the Resource Groups console, or pass them as arguments to Resource Groups commands in the AWS CLI\. With Resource Groups, you can create a custom console that organizes and consolidates information based on criteria that you specify in tags\. You can also use groups as the basis for viewing monitoring and configuration insights in AWS Systems Manager\.

------
#### [ AWS AppConfig ]

[AppConfig](appconfig.md) helps you create, manage, and quickly deploy application configurations\. AppConfig supports controlled deployments to applications of any size\. You can use AppConfig with applications hosted on EC2 instances, AWS Lambda, containers, mobile applications, or IoT devices\. To prevent errors when deploying application configurations, AppConfig includes validators\. A validator provides a syntactic or semantic check to ensure that the configuration you want to deploy works as intended\. During a configuration deployment, AppConfig monitors the application to ensure that the deployment is successful\. If the system encounters an error or if the deployment triggers an alarm, AppConfig rolls back the change to minimize impact for your application users\.

------
#### [ Parameter Store ]

[Parameter Store](systems-manager-parameter-store.md) provides secure, hierarchical storage for configuration data and secrets management\. You can store data such as passwords, database strings, EC2 instance IDs and Amazon Machine Image \(AMI\) IDs, and license codes as parameter values\. You can store values as plain text or encrypted data\. You can then reference values by using the unique name you specified when you created the parameter\.

------

## Actions & Change<a name="features-actions-and-change"></a>

Systems Manager provides the following capabilities for taking action against or changing your AWS resources\. Choose the tabs to learn more\.

------
#### [ Automation ]

Use [Systems Manager Automation](systems-manager-automation.md) to automate common maintenance and deployment tasks\. You can use Automation to create and update Amazon Machine Images, apply driver and agent updates, reset passwords on Windows Server instance, reset SSH keys on Linux instances, and apply OS patches or application updates\. 

------
#### [ Change Calendar ]

[Change Calendar](systems-manager-change-calendar.md) lets you set up date and time ranges when actions you specify \(for example, in [Systems Manager Automation](systems-manager-automation.md) documents\) may or may not be performed in your AWS account\. In Change Calendar, these ranges are called *events*\. When you create a Change Calendar entry, you are creating a [Systems Manager document](sysman-ssm-docs.md) of the type `ChangeCalendar`\. In Change Calendar, the document stores [iCalendar 2\.0](https://icalendar.org/) data in plaintext format\. Events that you add to the Change Calendar entry become part of the document\.

------
#### [ Maintenance Windows ]

Use [Maintenance Windows](systems-manager-maintenance.md) to set up recurring schedules for managed instances to run administrative tasks like installing patches and updates without interrupting business\-critical operations\. 

------

## Instances & Nodes<a name="features-instances-and-nodes"></a>

Systems Manager provides the following capabilities for managing your EC2 instances, your on\-premises servers and virtual machines \(VMs\) in your hybrid environment, and other types of AWS resources \(nodes\)\. Choose the tabs to learn more\.

------
#### [ Compliance ]

Use [Systems Manager Configuration Compliance](systems-manager-compliance.md) to scan your fleet of managed instances for patch compliance and configuration inconsistencies\. You can collect and aggregate data from multiple AWS accounts and Regions, and then drill down into specific resources that aren’t compliant\. By default, Configuration Compliance displays compliance data about Patch Manager patching and State Manager associations\. You can also customize the service and create your own compliance types based on your IT or business requirements\.

------
#### [ Inventory ]

[Inventory Manager](systems-manager-inventory.md) automates the process of collecting software inventory from managed instances\. You can use Inventory Manager to gather metadata about applications, files, components, patches, and more on your managed instances\.

------
#### [ Managed Instances ]

A [managed instance](systems-manager-setting-up.md) is any EC2 instance or on\-premises machine–a server or a virtual machine \(VM\)–in your hybrid environment that is configured for Systems Manager\. To set up managed instances, you need to install SSM Agent on your machines \(if not installed by default\) and configure AWS Identity and Access Management \(IAM\) permissions\. On\-premises machines also require an activation code\.

------
#### [ Hybrid Activations ]

To set up servers and VMs in your hybrid environment as managed instances, you need to create a managed\-instance [activation](systems-manager-managedinstances.md)\. After you complete the activation, you receive an activation code and ID\. This code/ID combination functions like an Amazon EC2 access ID and secret key to provide secure access to the Systems Manager service from your managed instances\.

------
#### [ Session Manager ]

Use [Session Manager](session-manager.md) to manage your EC2 instances through an interactive one\-click browser\-based shell or through the AWS CLI\. Session Manager provides secure and auditable instance management without the need to open inbound ports, maintain bastion hosts, or manage SSH keys\. Session Manager also makes it easy to comply with corporate policies that require controlled access to instances, strict security practices, and fully auditable logs with instance access details, while still providing end users with simple one\-click cross\-platform access to your EC2 instances\. 

------
#### [ Run Command ]

Use [Systems Manager Run Command](execute-remote-commands.md) to remotely and securely manage the configuration of your managed instances at scale\. Use Run Command to perform on\-demand changes like updating applications or running Linux shell scripts and Windows PowerShell commands on a target set of dozens or hundreds of instances\. 

------
#### [ State Manager ]

Use [Systems Manager State Manager](systems-manager-state.md) to automate the process of keeping your managed instances in a defined state\. You can use State Manager to ensure that your instances are bootstrapped with specific software at startup, joined to a Windows domain \(Windows Server instances only\), or patched with specific software updates\. 

------
#### [ Patch Manager ]

Use [Patch Manager](systems-manager-patch.md) to automate the process of patching your managed instances with both security related and other types of updates\. You can use Patch Manager to apply patches for both operating systems and applications\. \(On Windows Server, application support is limited to updates for Microsoft applications\.\) This capability enables you to scan instances for missing patches and apply missing patches individually or to large groups of instances by using EC2 instance tags\. Patch Manager uses *patch baselines*, which can include rules for auto\-approving patches within days of their release, as well as a list of approved and rejected patches\. You can install security patches on a regular basis by scheduling patching to run as a Systems Manager maintenance window task, or you can patch your managed instances on demand at any time\. For Linux operating systems, you can define the repositories that should be used for patching operations as part of your patch baseline\. This allows you to ensure that updates are installed only from trusted repositories regardless of what repositories are configured on the instance\. For Linux, you also have the ability to update any package on the instance, not just those that are classified as operating system security updates\. For Windows Server, you can also use Patch Manager to update supported Microsoft applications\.

------
#### [ Distributor ]

Use [Distributor](distributor.md) to create and deploy packages to managed instances\. Distributor lets you package your own software—or find AWS\-provided agent software packages, such as **AmazonCloudWatchAgent**—to install on AWS Systems Manager managed instances\. After you install a package for the first time, you can use Distributor to completely uninstall and reinstall a new package version, or perform an in\-place update that adds new or changed files only\. Distributor publishes resources, such as software packages, to AWS Systems Manager managed instances\.

------

## Shared Resources<a name="features-shared"></a>

Systems Manager uses the following shared resources for managing and configuring your AWS resources\. Choose the tabs to learn more\.

------
#### [ Documents ]

A [Systems Manager document](sysman-ssm-docs.md) \(SSM document\) defines the actions that Systems Manager performs\. SSM document types include *Command* documents, which are used by State Manager and Run Command, and *Automation* documents, which are used by Systems Manager Automation\. Systems Manager includes dozens of pre\-configured documents that you can use by specifying parameters at runtime\. Documents can be expressed in JSON or YAML, and include steps and parameters that you specify\.

------