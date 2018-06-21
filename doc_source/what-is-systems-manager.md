# What Is AWS Systems Manager?<a name="what-is-systems-manager"></a>

AWS Systems Manager is a collection of capabilities for configuring and managing your Amazon EC2 instances, on\-premises servers and virtual machines, and other AWS resources at scale\. Systems Manager includes a unified interface that allows you to easily centralize operational data and automate tasks across your AWS resources\. Systems Manager shortens the time to detect and resolve operational problems in your infrastructure\. Systems Manager gives you a complete view of your infrastructure performance and configuration, simplifies resource and application management, and makes it easy to operate and manage your infrastructure at scale\. 

**Note**  
AWS Systems Manager was formerly known as "Amazon EC2 Systems Manager" and "Amazon Simple Systems Manager"\. 

## How It Works<a name="how-it-works"></a>

Diagram 1 shows a general example of the different processes that Systems Manager performs when executing an action like sending a command to your fleet of servers or performing an inventory of the applications running on your on\-premises servers\. Each Systems Manager capability, for example Run Command or Maintenance Windows, uses a similar process of set up, execution, processing, and reporting\. 

1. **Configure Systems Manager**: Use the Systems Manager console, SDK, AWS CLI, or AWS Toolkit for Windows PowerShell to configure, schedule, automate, and execute actions that you want to perform on your AWS resources\. 

1. **Verification and processing**: Systems Manager verifies the configurations, including permissions, and sends requests to the SSM Agent running on your instances or servers in your hybrid environment\. SSM Agent performs the specified configuration changes\.

1. **Reporting**: SSM Agent reports the status of the configuration changes and actions to Systems Manager in the AWS cloud\. Systems Manager then sends the status to the user and various AWS services, if configured\.

**Diagram 1: General Example of Systems Manager Process Flow**

![\[Diagram showing how Systems Manager capabilities, for example Run Command or Maintenance Windows, use a similar process of set up, execution, processing, and reporting.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/how-it-works.png)

## Capabilities<a name="features"></a>

Systems Manager includes the following capabilities:

**Topics**
+ [Resource Groups](#features-rg)
+ [Insights](#features-insights)
+ [Actions](#features-actions)
+ [Shared Resources](#features-shared)

### Resource Groups<a name="features-rg"></a>

[AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html): An AWS *resource* is an entity you can work with in AWS, such as an Amazon Elastic Compute Cloud \(Amazon EC2\) instance, an Amazon Elastic Block Store \(Amazon EBS\) volume, a security group, or an Amazon Virtual Private Cloud \(VPC\)\. A *resource group* is a collection of AWS resources that are all in the same AWS region, and that match criteria provided in a query\. You build queries in the Resource Groups console, or pass them as arguments to Resource Groups commands in the AWS CLI\. With Resource Groups, you can create a custom console that organizes and consolidates information based on criteria that you specify in tags\. You can also use groups as the basis for viewing monitoring and configuration insights in AWS Systems Manager\.

### Insights<a name="features-insights"></a>

Systems Manager provides the following capabilities for centrally viewing data about your AWS resources\. Choose the tabs to learn more\.

------
#### [ Built\-in Insights ]

[Insights](https://docs.aws.amazon.com/ARG/latest/userguide/viewing-group-insights.html) show detailed information about the resources in your AWS Resource Groups, such as AWS CloudTrail logs, results of evaluations against AWS Config rules, and AWS Trusted Advisor reports\. Insights show information about a single, selected resource group at a time\.

------
#### [ CloudWatch Dashboards ]

[Amazon CloudWatch Dashboards](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html) are customizable home pages in the CloudWatch console that you can use to monitor your resources in a single view, even those resources that are spread across different regions\. You can use CloudWatch dashboards to create customized views of the metrics and alarms for your AWS resources\. 

------
#### [ Inventory Management ]

[Inventory Manager](systems-manager-inventory.md) automates the process of collecting software inventory from managed instances\. You can use Inventory Manager to gather metadata about applications, files, components, patches, and more on your managed instances\.

------
#### [ Configuration Compliance ]

Use [Systems Manager Configuration Compliance](systems-manager-compliance.md) to scan your fleet of managed instances for patch compliance and configuration inconsistencies\. You can collect and aggregate data from multiple AWS accounts and Regions, and then drill down into specific resources that aren’t compliant\. By default, Configuration Compliance displays compliance data about Patch Manager patching and State Manager associations\. You can also customize the service and create your own compliance types based on your IT or business requirements\.

------

### Actions<a name="features-actions"></a>

Systems Manager provides the following capabilities for taking action against your AWS resources\. Choose the tabs to learn more\.

------
#### [ Automation ]

Use [Systems Manager Automation](systems-manager-automation.md) to automate common maintenance and deployment tasks\. You can use Automation to create and update Amazon Machine Images, apply driver and agent updates, reset passwords on Windows instance, reset SSH keys on Linux instances, and apply OS patches or application updates\. 

------
#### [ Run Command ]

Use [Systems Manager Run Command](execute-remote-commands.md) to remotely and securely manage the configuration of your managed instances at scale\. Use Run Command to perform on\-demand changes like updating applications or running Linux shell scripts and Windows PowerShell commands on a target set of dozens or hundreds of instances\. 

------
#### [ Patch Management ]

Use [Patch Manager](systems-manager-patch.md) to automate the process of patching your managed instances\. This capability enables you to scan instances for missing patches and apply missing patches individually or to large groups of instances by using Amazon EC2 instance tags\. For security patches, Patch Manager uses patch baselines that include rules for auto\-approving patches within days of their release, as well as a list of approved and rejected patches\. Security patches are installed from the default repository for patches configured for the instance\. You can install security patches on a regular basis by scheduling patching to run as a Systems Manager Maintenance Window task\. For Linux operating systems, you can define the repositories that should be used for patching operations as part of your patch baseline\. This allows you to ensure that updates are installed only from trusted repositories regardless of what repositories are configured on the instance\. For Linux, you also have the ability to update any package on the instance, not just those that are classified as operating system security updates\.

------
#### [ Maintenance Windows ]

Use [Maintenance Windows](systems-manager-maintenance.md) to set up recurring schedules for managed instances to execute administrative tasks like installing patches and updates without interrupting business\-critical operations\. 

------
#### [ State Management ]

Use [Systems Manager State Manager](systems-manager-state.md) to automate the process of keeping your managed instances in a defined state\. You can use State Manager to ensure that your instances are bootstrapped with specific software at startup, joined to a Windows domain \(Windows instances only\), or patched with specific software updates\. 

------

### Shared Resources<a name="features-shared"></a>

Systems Manager uses the following shared resources for managing and configuring your AWS resources\. Choose the tabs to learn more\.

------
#### [ Managed Instances ]

A [managed instances](systems-manager-setting-up.md) is any Amazon EC2 instance or on\-premises machine \(server or virtual machine \[VM\]\) in your hybrid environment that is configured for Systems Manager\. To set up managed instances, you need to install SSM agent on your machines \(if not installed by default\) and configure AWS Identity and Access Management \(IAM\) permissions\. On\-premises machines also require an activation code\.

------
#### [ Activations ]

To set up servers and VMs in your hybrid environment as managed instances, you need to create a managed\-instance [activation](systems-manager-managedinstances.md)\. After you complete the activation, you receive an activation code and ID\. This code/ID combination functions like an Amazon EC2 access ID and secret key to provide secure access to the Systems Manager service from your managed instances\.

------
#### [ Systems Manager Documents ]

A [Systems Manager document](sysman-ssm-docs.md) \(SSM document\) defines the actions that Systems Manager performs\. SSM documents can be either *Command* documents, which are used by State Manager and Run Command, or Automation documents, which are used by Systems Manager Automation\. Systems Manager includes more dozens of pre\-configured documents that you can use by specifying parameters at runtime\. Documents can be expressed in JSON or YAML, and include steps and parameters that you specify\.

------
#### [ Parameter Store ]

[Parameter Store](systems-manager-paramstore.md) provides secure, hierarchical storage for configuration data and secrets management\. You can store data such as passwords, database strings, and license codes as parameter values\. You can store values as plain text or encrypted data\. You can then reference values by using the unique name you specified when you created the parameter\.

------

## Getting Started<a name="sysman-gstarted"></a>

To get started with Systems Manager, do the following:
+ Ensure you have completed the Systems Manager prerequisites
+ Configure roles and permissions
+ Install SSM Agent on your instance \(if necessary\)

If you want to manage your on\-premises servers and VMs with Systems Manager, then you must also create a managed instance activation\.

These tasks are described in [Setting Up AWS Systems Manager](systems-manager-setting-up.md)\.

## Accessing Systems Manager<a name="access-methods"></a>

You can access Systems Manager using any of the following interfaces:
+ The [AWS Systems Manager console](https://console.aws.amazon.com/systems-manager/) — Provides a web interface that you can use to access Systems Manager\.
+ **AWS Command Line Interface \(AWS CLI\)** — Provides commands for a broad set of AWS services, including Systems Manager, and is supported on Windows, Mac, and Linux\. For more information, see [AWS Command Line Interface](https://aws.amazon.com/cli/)\.
+ **AWS Tools for Windows PowerShell** — Provides commands for a broad set of AWS services, including Systems Manager\. For more information, see [AWS Tools for Windows PowerShell](https://aws.amazon.com/powerhshell/)\.
+ **AWS SDKs** — Provides language\-specific APIs and takes care of many of the connection details, such as calculating signatures, handling request retries, and error handling\. For more information, see [AWS SDKs](http://aws.amazon.com/tools/#SDKs)\.

## Pricing<a name="pricing"></a>

Systems Manager features and shared components are offered at no additional cost\. You pay only for the AWS resources that you use\.

## We Want to Hear from You<a name="welcome-contact-us"></a>

We welcome your feedback\. To contact us, visit [the AWS Systems Manager forum](https://forums.aws.amazon.com//forum.jspa?forumID=185)\.

## Related Content<a name="related-content"></a>

Systems Manager is also documented in the following references\.
+ [Amazon EC2 Systems Manager API Reference](http://docs.aws.amazon.com/ssm/latest/APIReference/)
+ [Systems Manager AWS Tools for Windows PowerShell](http://docs.aws.amazon.com/powershell/latest/reference/items/Amazon_Simple_Systems_Management_cmdlets.html)
+ [Systems Manager AWS CLI Reference](http://docs.aws.amazon.com/cli/latest/reference/ssm/index.html)
+  [AWS SDKs](http://aws.amazon.com/tools/#SDKs)
+ [AWS Systems Manager Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_ssm)