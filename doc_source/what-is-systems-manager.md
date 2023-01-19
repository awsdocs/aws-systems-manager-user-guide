# What is AWS Systems Manager?<a name="what-is-systems-manager"></a>

AWS Systems Manager is the operations hub for your AWS applications and resources and a secure end\-to\-end management solution for hybrid cloud environments that enables secure operations at scale\.

## How Systems Manager works<a name="how-it-works"></a>

The following diagram describes how some Systems Manager capabilities perform actions on your resources\. The diagram doesn't cover all capabilities\. Each enumerated interaction is described before the diagram\.

1. **Access Systems Manager** – Use one of the available options for [accessing Systems Manager](#access-methods)\.

1. **Choose a Systems Manager capability** – Determine which capability can help you perform the action you want to perform on your resources\. The diagram shows only a few of the capabilities that IT administrators and DevOps personnel use to manage their applications and resources\.

1. **Verification and processing** – Systems Manager verifies that your AWS Identity and Access Management \(IAM\) user, group, or role has permission to perform the action you specified\. If the target of your action is a managed node, the Systems Manager Agent \(SSM Agent\) running on the node performs the action\. For other types of resources, Systems Manager performs the specified action or communicates with other AWS services to perform the action on behalf of Systems Manager\.

1. **Reporting** – Systems Manager, SSM Agent, and other AWS services that performed an action on behalf of Systems Manager report status\. Systems Manager can send status details to other AWS services, if configured\.

1. **Systems Manager operations management capabilities** – If enabled, Systems Manager operations management capabilities such as Explorer, OpsCenter, and Incident Manager aggregate operations data or create artifacts in response to events or errors with your resources\. These artifacts include operational work items \(OpsItems\) and incidents\. Systems Manager operations management capabilities provide operational insight into your applications and resources and automated remediation solutions to help troubleshoot problems\.

![\[Systems Manager capabilities perform actions on your resources.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/how-it-works.png)

## Systems Manager capabilities<a name="systems-manager-capabilities"></a>

Systems Manager groups capabilities into the following categories\. Choose the tabs under each category to learn more about each capability\.

**Topics**
+ [Application management](#systems-manager-application-management)
+ [Change management](#capabilities-actions-and-change)
+ [Node management](#capabilities-instances-and-nodes)
+ [Operations management](#capabilities-operations-management)
+ [Quick Setup](#capabilities-quick-setup)
+ [Shared resources](#capabilities-shared)

### Application management<a name="systems-manager-application-management"></a>

------
#### [ Application Manager ]

[Application Manager](application-manager.md) helps DevOps engineers investigate and remediate issues with their AWS resources in the context of their applications and clusters\. In Application Manager, an *application* is a logical group of AWS resources that you want to operate as a unit\. This logical group can represent different versions of an application, ownership boundaries for operators, or developer environments, to name a few\. Application Manager support for container clusters includes both Amazon Elastic Kubernetes Service \(Amazon EKS\) and Amazon Elastic Container Service \(Amazon ECS\) clusters\. Application Manager aggregates operations information from multiple AWS services and Systems Manager capabilities to a single AWS Management Console\.

------
#### [ AppConfig ]

[AppConfig](appconfig.md) helps you create, manage, and deploy application configurations and feature flags\. AppConfig supports controlled deployments to applications of any size\. You can use AppConfig with applications hosted on Amazon EC2 instances, AWS Lambda containers, mobile applications, or edge devices\. To prevent errors when deploying application configurations, AppConfig includes validators\. A validator provides a syntactic or semantic check to verify that the configuration you want to deploy works as intended\. During a configuration deployment, AppConfig monitors the application to verify that the deployment is successful\. If the system encounters an error or if the deployment invokes an alarm, AppConfig rolls back the change to minimize impact for your application users\.

------
#### [ Parameter Store ]

[Parameter Store](systems-manager-parameter-store.md) provides secure, hierarchical storage for configuration data and secrets management\. You can store data such as passwords, database strings, Amazon Elastic Compute Cloud \(Amazon EC2\) instance IDs and Amazon Machine Image \(AMI\) IDs, and license codes as parameter values\. You can store values as plain text or encrypted data\. You can then reference values by using the unique name you specified when you created the parameter\.

------

### Change management<a name="capabilities-actions-and-change"></a>

------
#### [ Change Manager ]

[Change Manager](change-manager.md) is an enterprise change management framework for requesting, approving, implementing, and reporting on operational changes to your application configuration and infrastructure\. From a single *delegated administrator account*, if you use AWS Organizations, you can manage changes across multiple AWS accounts in multiple AWS Regions\. Alternatively, using a *local account*, you can manage changes for a single AWS account\. Use Change Manager for managing changes to both AWS resources and on\-premises resources\.

------
#### [ Automation ]

Use [Automation](systems-manager-automation.md) to automate common maintenance and deployment tasks\. You can use Automation to create and update Amazon Machine Images \(AMIs\), apply driver and agent updates, reset passwords on Windows Server instance, reset SSH keys on Linux instances, and apply OS patches or application updates\. 

------
#### [ Change Calendar ]

[Change Calendar](systems-manager-change-calendar.md) helps you set up date and time ranges when actions you specify \(for example, in [Systems Manager Automation](systems-manager-automation.md) runbooks\) can or can't be performed in your AWS account\. In Change Calendar, these ranges are called *events*\. When you create a Change Calendar entry, you're creating a [Systems Manager document](sysman-ssm-docs.md) of the type `ChangeCalendar`\. In Change Calendar, the document stores [iCalendar 2\.0](https://icalendar.org/) data in plaintext format\. Events that you add to the Change Calendar entry become part of the document\. You can add events manually in the Change Calendar interface or import events from a supported third\-party calendar using an `.ics` file\.

------
#### [ Maintenance Windows ]

Use [Maintenance Windows](systems-manager-maintenance.md) to set up recurring schedules for managed instances to run administrative tasks such as installing patches and updates without interrupting business\-critical operations\. 

------

### Node management<a name="capabilities-instances-and-nodes"></a>

A *managed node* is any machine configured for Systems Manager\. Systems Manager supports Amazon Elastic Compute Cloud \(Amazon EC2\) instances, edge devices, and on\-premises servers or virtual machines \(VMs\), including VMs in other cloud environments\.

------
#### [ Compliance ]

Use [Compliance](systems-manager-compliance.md) to scan your fleet of managed nodes for patch compliance and configuration inconsistencies\. You can collect and aggregate data from multiple AWS accounts and AWS Regions, and then drill down into specific resources that aren’t compliant\. By default, Compliance displays compliance data about Patch Manager patching and State Manager associations\. You can also customize the service and create your own compliance types based on your IT or business requirements\.

------
#### [ Fleet Manager ]

[Fleet Manager](fleet.md) is a unified user interface \(UI\) experience that helps you remotely manage your nodes\. With Fleet Manager, you can view the health and performance status of your entire fleet from one console\. You can also gather data from individual devices and instances to perform common troubleshooting and management tasks from the console\. This includes viewing directory and file contents, Windows registry management, operating system user management, and more\. 

------
#### [ Inventory ]

[Inventory](systems-manager-inventory.md) automates the process of collecting software inventory from your managed nodes\. You can use Inventory to gather metadata about applications, files, components, patches, and more\.

------
#### [ Session Manager ]

Use [Session Manager](session-manager.md) to manage your edge devices and Amazon Elastic Compute Cloud \(Amazon EC2\) instances through an interactive one\-click browser\-based shell or through the AWS CLI\. Session Manager provides secure and auditable edge device and instance management without needing to open inbound ports, maintain bastion hosts, or manage SSH keys\. Session Manager also allows you to comply with corporate policies that require controlled access to edge devices and instances, strict security practices, and fully auditable logs with edge device and instance access details, while still providing end users with simple one\-click cross\-platform access to your edge devices and EC2 instances\. To use Session Manager, you must enable the advanced\-instances tier\. For more information, see [Turning on the advanced\-instances tier](systems-manager-managedinstances-advanced.md)\.

------
#### [ Run Command ]

Use [Run Command](run-command.md) to remotely and securely manage the configuration of your managed nodes at scale\. Use Run Command to perform on\-demand changes such as updating applications or running Linux shell scripts and Windows PowerShell commands on a target set of dozens or hundreds of managed nodes\. 

------
#### [ State Manager ]

Use [State Manager](systems-manager-state.md) to automate the process of keeping your managed nodes in a defined state\. You can use State Manager to guarantee that your managed nodes are bootstrapped with specific software at startup, joined to a Windows domain \(Windows Server nodes only\), or patched with specific software updates\. 

------
#### [ Patch Manager ]

Use [Patch Manager](systems-manager-patch.md) to automate the process of patching your managed nodes with both security related and other types of updates\. You can use Patch Manager to apply patches for both operating systems and applications\. \(On Windows Server, application support is limited to updates for applications released by Microsoft\.\)

This capability allows you to scan managed nodes for missing patches and apply missing patches individually or to large groups of managed nodes by using tags\. Patch Manager uses *patch baselines*, which can include rules for auto\-approving patches within days of their release, and a list of approved and rejected patches\. You can install security patches on a regular basis by scheduling patching to run as a Systems Manager maintenance window task, or you can patch your managed nodes on demand at any time\.

For Linux operating systems, you can define the repositories that should be used for patching operations as part of your patch baseline\. This allows you to ensure that updates are installed only from trusted repositories regardless of what repositories are configured on the managed node\. For Linux, you also have the ability to update any package on the managed node, not just those that are classified as operating system security updates\. You can also generate patch reports that are sent to an S3 bucket of your choice\. For a single managed node, reports include details of all patches for the machine\. For a report on all managed nodes, only a summary of how many patches are missing is provided\.

------
#### [ Distributor ]

Use [Distributor](distributor.md) to create and deploy packages to managed nodes\. With Distributor, you can package your own software—or find AWS\-provided agent software packages, such as **AmazonCloudWatchAgent**—to install on Systems Manager managed nodes\. After you install a package for the first time, you can use Distributor to uninstall and reinstall a new package version, or perform an in\-place update that adds new or changed files\. Distributor publishes resources, such as software packages, to Systems Manager managed nodes\.

------
#### [ Hybrid Activations ]

To set up servers and VMs in your hybrid environment as managed instances, create a managed instance [activation](systems-manager-managedinstances.md)\. After you complete the activation, you receive an activation code and ID\. This code and ID combination functions like an Amazon Elastic Compute Cloud \(Amazon EC2\) access ID and secret key to provide secure access to the Systems Manager service from your managed instances\.

You can also create an activation for edge devices if you want to manage them by using Systems Manager\.

------

### Operations management<a name="capabilities-operations-management"></a>

------
#### [ Incident Manager ]

[Incident Manager](incident-manager.md) is an incident management console that helps users mitigate and recover from incidents affecting their AWS hosted applications\.

Incident Manager increases incident resolution by notifying responders of impact, highlighting relevant troubleshooting data, and providing collaboration tools to get services back up and running\. Incident Manager also automates response plans and allows responder team escalation\. 

------
#### [ Explorer ]

[Explorer](Explorer.md) is a customizable operations dashboard that reports information about your AWS resources\. Explorer displays an aggregated view of operations data \(OpsData\) for your AWS accounts and across AWS Regions\. In Explorer, OpsData includes metadata about your Amazon EC2 instances, patch compliance details, and operational work items \(OpsItems\)\. Explorer provides context about how OpsItems are distributed across your business units or applications, how they trend over time, and how they vary by category\. You can group and filter information in Explorer to focus on items that are relevant to you and that require action\. When you identify high priority issues, you can use OpsCenter, a capability of Systems Manager, to run Automation runbooks and resolve those issues\. 

------
#### [ OpsCenter ]

[OpsCenter](OpsCenter.md) provides a central location where operations engineers and IT professionals can view, investigate, and resolve operational work items \(OpsItems\) related to AWS resources\. OpsCenter is designed to reduce mean time to resolution for issues impacting AWS resources\. This Systems Manager capability aggregates and standardizes OpsItems across services while providing contextual investigation data about each OpsItem, related OpsItems, and related resources\. OpsCenter also provides Systems Manager Automation runbooks that you can use to resolve issues\. You can specify searchable, custom data for each OpsItem\. You can also view automatically generated summary reports about OpsItems by status and source\. 

------
#### [ CloudWatch Dashboards ]

[Amazon CloudWatch Dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) are customizable pages in the CloudWatch console that you can use to monitor your resources in a single view, even those resources that are spread across different regions\. You can use CloudWatch dashboards to create customized views of the metrics and alarms for your AWS resources\. 

------

### Quick Setup<a name="capabilities-quick-setup"></a>

Use [Quick Setup](systems-manager-quick-setup.md) to configure frequently used AWS services and features with recommended best practices\. You can use Quick Setup in an individual AWS account or across multiple AWS accounts and AWS Regions by integrating with AWS Organizations\. Quick Setup simplifies setting up services, including Systems Manager, by automating common or recommended tasks\. These tasks include, for example, creating required AWS Identity and Access Management \(IAM\) instance profile roles and setting up operational best practices, such as periodic patch scans and inventory collection\.

### Shared resources<a name="capabilities-shared"></a>

------
#### [ Documents ]

A [Systems Manager document](sysman-ssm-docs.md) \(SSM document\) defines the actions that Systems Manager performs\. SSM document types include *Command* documents, which are used by State Manager and Run Command, and Automation runbooks, which are used by Systems Manager Automation\. Systems Manager includes dozens of pre\-configured documents that you can use by specifying parameters at runtime\. Documents can be expressed in JSON or YAML, and include steps and parameters that you specify\.

------

## Accessing Systems Manager<a name="access-methods"></a>

You can work with Systems Manager in any of the following ways:

**Systems Manager console**  
The [Systems Manager console](https://console.aws.amazon.com/systems-manager/) is a browser\-based interface to access and use Systems Manager\.

**AWS IoT Greengrass V2 console**  
You can view and manage edge devices that are configured for AWS IoT Greengrass in the [Greengrass console](https://console.aws.amazon.com/iot)\.

**AWS command line tools**  
By using the AWS command line tools, you can issue commands at your system's command line to perform Systems Manager and other AWS tasks\. The tools are supported on Linux, macOS, and Windows\. Using the AWS Command Line Interface \(AWS CLI\) can be faster and more convenient than using the console\. The command line tools also are useful if you want to build scripts that perform AWS tasks\.   
AWS provides two sets of command line tools: the [AWS Command Line Interface](http://aws.amazon.com/cli/) and the [AWS Tools for Windows PowerShell](http://aws.amazon.com/powershell/)\. For information about installing and using the AWS CLI, see the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)\. For information about installing and using the Tools for Windows PowerShell, see the [AWS Tools for Windows PowerShell User Guide](https://docs.aws.amazon.com/powershell/latest/userguide/)\.  
On your Windows Server instances, Windows PowerShell 3\.0 or later is required to run certain SSM documents \(for example, the legacy `AWS-ApplyPatchBaseline` document\)\. Verify that your Windows Server instances are running Windows Management Framework 3\.0 or later\. The framework includes Windows PowerShell\.

**AWS SDKs**  
AWS provides software development kits \(SDKs\) that consist of libraries and sample code for various programming languages and platforms \(for example, [Java](http://aws.amazon.com/sdk-for-java/), [Python](http://aws.amazon.com/sdk-for-python/), [Ruby](http://aws.amazon.com/sdk-for-ruby/), [\.NET](http://aws.amazon.com/sdk-for-net/), [iOS and Android](http://aws.amazon.com/mobile/resources/), and [others](http://aws.amazon.com/tools/#sdk)\)\. The SDKs provide a convenient way to create programmatic access to Systems Manager\. For information about the AWS SDKs, including how to download and install them, see [Tools for Amazon Web Services](http://aws.amazon.com/tools/#sdk)\.

## Systems Manager service name history<a name="service-naming-history"></a>

AWS Systems Manager \(Systems Manager\) was formerly known as "Amazon Simple Systems Manager \(SSM\)" and "Amazon EC2 Systems Manager \(SSM\)"\. The original abbreviated name of the service, "SSM", is still reflected in various AWS resources, including a few other service consoles\. Some examples:
+ **Systems Manager Agent**: SSM Agent
+ **Systems Manager parameters**: SSM parameters
+ **Systems Manager service endpoints**: `ssm.region.amazonaws.com`
+ **AWS CloudFormation resource types**: `AWS::SSM::Document`
+ **AWS Config rule identifier**: `EC2_INSTANCE_MANAGED_BY_SSM`
+ **AWS Command Line Interface \(AWS CLI\) commands**: `aws ssm describe-patch-baselines`
+ **AWS Identity and Access Management \(IAM\) managed policy names**: `AmazonSSMReadOnlyAccess`
+ **Systems Manager resource ARNs**: `arn:aws:ssm:region:account-id:patchbaseline/pb-07d8884178EXAMPLE`

## Supported AWS Regions<a name="systems-manager-supported-Regions"></a>

Systems Manager is available in the AWS Regions listed in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\. Before starting your Systems Manager configuration process, we recommend that you verify the service is available in each of the AWS Regions you want to use it in\. 

For on\-premises servers and VMs in your hybrid environment, we recommend that you choose the Region closest to your data center or computing environment\.

## Systems Manager pricing<a name="systems-manager-pricing"></a>

Some Systems Manager capabilities charge a fee\. For more information, see [AWS Systems Manager Pricing](http://aws.amazon.com/systems-manager/pricing/)\.