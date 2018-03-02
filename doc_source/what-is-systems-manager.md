# What Is AWS Systems Manager?<a name="what-is-systems-manager"></a>

AWS Systems Manager \(formerly Amazon EC2 Systems Manager\) is a unified interface that allows you to easily centralize operational data and automate tasks across your AWS resources\. Systems Manager shortens the time to detect and resolve operational problems in your infrastructure\. Systems Manager gives you a complete view of your infrastructure performance and configuration, simplifies resource and application management, and makes it easy to operate and manage your infrastructure at scale\. 

## Features<a name="features"></a>

Systems Manager includes the following features:


+ [Resource Groups](#features-rg)
+ [Insights](#features-insights)
+ [Actions](#features-actions)
+ [Shared Resources](#features-shared)

### Resource Groups<a name="features-rg"></a>

[AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/welcome.html): A resource group is a collection of AWS resources that are all in the same AWS region, and that match criteria provided in a query\. You build queries in the Resource Groups console, or pass them as arguments to Resource Groups commands in the AWS CLI\. With Resource Groups, you can create a custom console that organizes and consolidates information based on criteria that you specify in tags\. After you've created groups in Resource Groups, use AWS Systems Manager tools such as Automation, Run Command, Patch Manager, and Maintenance Windows to simplify management tasks on your groups of resources\. You can also use groups as the basis for viewing monitoring and configuration insights in AWS Systems Manager\.

### Insights<a name="features-insights"></a>

Systems Manager provides the following features and capabilities for centrally viewing data about your AWS resources\.

+ [Built\-in Insights](https://docs.aws.amazon.com/ARG/latest/userguide/viewing-group-insights.html): Insights show detailed information about the resources in your AWS Resource Groups, such as AWS CloudTrail logs, results of evaluations against AWS Config rules, and AWS Trusted Advisor reports\. Insights show information about a single, selected resource group at a time\.

+ [Amazon CloudWatch Dashboard](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html): CloudWatch dashboards are customizable home pages in the CloudWatch console that you can use to monitor your resources in a single view, even those resources that are spread across different regions\. You can use CloudWatch dashboards to create customized views of the metrics and alarms for your AWS resources\. 

+ [Inventory Management](systems-manager-inventory.md): Inventory Manager automates the process of collecting software inventory from managed instances\. You can use Inventory Manager to gather metadata about OS and system configurations and application deployments\.

+ [Configuration Compliance](systems-manager-compliance.md): Use Configuration Compliance to scan your fleet of managed instances for patch compliance and configuration inconsistencies\. You can collect and aggregate data from multiple AWS accounts and Regions, and then drill down into specific resources that aren’t compliant\. By default, Configuration Compliance displays compliance data about Patch Manager patching and State Manager associations\. You can also customize the service and create your own compliance types based on your IT or business requirements\.

### Actions<a name="features-actions"></a>

Systems Manager provides the following capabilities for taking action against your AWS resources\.

+ [Automation](systems-manager-automation.md): Automation automates common maintenance and deployment tasks\. You can use Automation to create and update Amazon Machine Images, apply driver and agent updates, and apply OS patches or application updates\. 

+ [Run Command](execute-remote-commands.md): Run Command helps you remotely and securely manage the configuration of your managed instances at scale\. Use Run Command to perform on\-demand changes like updating applications or running Linux shell scripts and Windows PowerShell commands on a target set of dozens or hundreds of instances\. 

+ [Patch Management](systems-manager-patch.md): Patch Manager automates the process of patching your managed instances\. This feature enables you to scan instances for missing patches and apply missing patches individually or to large groups of instances by using Amazon EC2 instance tags\. For security patches, Patch Manager uses patch baselines that include rules for auto\-approving patches within days of their release, as well as a list of approved and rejected patches\. Security patches are installed from the default repository for patches configured for the instance\. You can install security patches on a regular basis by scheduling patching to run as a Systems Manager Maintenance Window task\. For non\-security patches \(Linux only\), you can create a patch baseline that uses a different patch repository that you configure and specify on the instance\. Then you can use Run Command to scan for or install the patches you specify for that non\-security update\. Installing non\-security updates from a configured repository doesn't change the default security repository for the instance\.

+ [Maintenance Windows](systems-manager-maintenance.md): Maintenance Windows let you set up recurring schedules for managed instances to execute administrative tasks like installing patches and updates without interrupting business\-critical operations\. 

+ [State Management](systems-manager-state.md): State Manager automates the process of keeping your managed instances in a defined state\. You can use State Manager to ensure that your instances are bootstrapped with specific software at startup, joined to a Windows domain \(Windows instances only\), or patched with specific software updates\. 

### Shared Resources<a name="features-shared"></a>

Systems Manager uses the following shared resources for managing and configuring your AWS resources\.

+ [Managed Instances](systems-manager-setting-up.md): A managed instance is any Amazon EC2 instance or on\-premise machine \(server or virtual machine \[VM\]\) in your hybrid environment that is configured for Systems Manager\. To set up managed instances, you need to install SSM agent on your machines \(if not installed by default\) and configure AWS Identity and Access Management \(IAM\) permissions\. On\-premises machines also require an activation code\.

+ [Activations](systems-manager-managedinstances.md): To set up servers and VMs in your hybrid environment as managed instances, you need to create a managed\-instance activation\. After you complete the activation, you receive an activation code and ID\. This code/ID combination functions like an Amazon EC2 access ID and secret key to provide secure access to the Systems Manager service from your managed instances\.

+ [Systems Manager Documents](sysman-ssm-docs.md): A Systems Manager document \(SSM document\) defines the actions that Systems Manager performs on your managed instances\. Systems Manager includes more than a dozen pre\-configured documents that you can use by specifying parameters at runtime\. Documents use JavaScript Object Notation \(JSON\) or YAML, and include steps and parameters that you specify\.

+ [Parameter Store](systems-manager-paramstore.md): Parameter Store provides secure, hierarchical storage for configuration data management and secrets management\. You can store data such as passwords, database strings, and license codes as parameter values\. You can store values as plain text or encrypted data\. You can then reference values by using the unique name you specified when you created the parameter\.

## Getting Started<a name="sysman-gstarted"></a>

To get started with Systems Manager, verify prerequisites, configure roles and permissions, and install the SSM Agent on your instances\. If you want to manage your on\-premises servers and VMs with Systems Manager, then you must also create a managed instance activation\. These tasks are described in [Setting Up AWS Systems Manager](systems-manager-setting-up.md)\.

## Accessing Systems Manager<a name="access-methods"></a>

You can access Systems Manager using any of the following interfaces:

+ The [AWS Systems Manager console](https://console.aws.amazon.com/systems-manager/) — Provides a web interface that you can use to access Systems Manager\.

+ **AWS Command Line Interface \(AWS CLI\)** — Provides commands for a broad set of AWS services, including Systems Manager, and is supported on Windows, Mac, and Linux\. For more information, see [AWS Command Line Interface](https://aws.amazon.com/cli/)\.

+ **AWS SDKs** — Provides language\-specific APIs and takes care of many of the connection details, such as calculating signatures, handling request retries, and error handling\. For more information, see [AWS SDKs](http://aws.amazon.com/tools/#SDKs)\.

+ **Query API**— Provides low\-level API actions that you call using HTTPS requests\. Using the Query API is the most direct way to access Systems Manager, but it requires that your application handle low\-level details such as generating the hash to sign the request, and error handling\. For more information, see the [Amazon EC2 Systems Manager API Reference](http://docs.aws.amazon.com/ssm/latest/APIReference/)\.

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