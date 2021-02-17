# Step 3: Try Systems Manager tutorials and walkthroughs<a name="getting-started-walkthroughs"></a>

This topic guides you to tutorials, walkthroughs, and basic tasks to help you learn how to use Systems Manager\.

Because Systems Manager is a collection of several capabilities, no single walkthrough or tutorial can introduce the entire service\. Therefore, we've provided links to resources according to the capability for which they provide practice\.

In most cases, you do not need to complete additional setup or configuration tasks before you start\. You can complete all of the tasks if you have necessary permissions and, where needed, access to one or more managed instances\.

In some cases, additional configuration, setup, or experience with Systems Manager are required before you try a tutorial or walkthrough\. We have identify those tutorials and walkthroughs as "Advanced"\.

**Topics**
+ [Operations Management](#getting-started-walkthroughs-operations-management)
+ [Application Management](#getting-started-walkthroughs-application-management)
+ [Change Management](#getting-started-walkthroughs-change-management)
+ [Node Management](#getting-started-walkthroughs-node-management)
+ [Shared Resources](#getting-started-walkthroughs-shared-resources)

## Operations Management<a name="getting-started-walkthroughs-operations-management"></a>

**Explorer**  
[AWS Systems Manager Explorer](Explorer.md) is a customizable operations dashboard that reports information about your AWS resources\. Explorer displays an aggregated view of operations data \(OpsData\) for your AWS accounts and across Regions\. 
+ [Customizing the display and using filters](Explorer-using-filters.md)

**OpsCenter**  
[AWS Systems Manager OpsCenter](OpsCenter.md) provides a central location where operations engineers and IT professionals can view, investigate, and resolve operational work items \(OpsItems\) related to AWS resources\.
+ [Configuring CloudWatch to create OpsItems from alarms](OpsCenter-create-OpsItems-from-CloudWatch-Alarms.md)
+ [Configuring EventBridge to automatically create OpsItems for specific events](OpsCenter-automatically-create-OpsItems-2.md)

## Application Management<a name="getting-started-walkthroughs-application-management"></a>

**Application Manager**  
[AWS Systems Manager Application Manager](application-manager.md) helps DevOps engineers investigate and remediate issues with their AWS resources in the context of their applications\. Application Manager aggregates operations information from multiple AWS services and Systems Manager capabilities to a single AWS Management Console\. 
+ [Viewing overview information about an application](application-manager-working-viewing-overview.md)

**Parameter Store**  
The [AWS Systems Manager Parameter Store](systems-manager-parameter-store.md) capability provides a centralized store to manage your configuration data, whether plain\-text data such as database strings or secrets such as passwords\. This allows you to separate your secrets and configuration data from your code\. Parameters can be tagged and organized into hierarchies, helping you manage parameters more easily\.
+ [Create a parameter \(console\)](parameter-create-console.md#param-create-console)
+ [Create a Systems Manager parameter \(AWS CLI\)](param-create-cli.md)
+ [Working with parameter hierarchies](sysman-paramstore-hierarchies.md)
+ Advanced: [Create a SecureString parameter and join an instance to a Domain \(PowerShell\)](sysman-paramstore-walk.md#sysman-param-securestring-walkthrough)

## Change Management<a name="getting-started-walkthroughs-change-management"></a>

**Change Manager**  
[AWS Systems Manager Change Manager](change-manager.md) is an enterprise change management framework for requesting, approving, implementing, and reporting on operational changes to your application configuration and infrastructure\.
+ Advanced: [Try out the AWS managed Hello World change template](change-templates-aws-managed.md)

**Automation**  
The [AWS Systems Manager Automation](systems-manager-automation.md) capability allows you to safely automate operations and management tasks across AWS resources\. You can automate common IT tasks, safely perform disruptive tasks in bulk, simplify complex tasks, enhance operations security, and used stored configuration scripts share best practices with the rest of your organization\.
+ Advanced: [Walkthrough: Patch a Linux AMI \(console\)](automation-walk-patch-linux-ami-console.md)
+ Advanced: [Walkthrough: Patch a Linux AMI \(AWS CLI\)](automation-walk-patch-linux-ami-cli.md)
+ Advanced: [Walkthrough: Patch a Windows Server AMI](automation-walk-patch-windows-ami-cli.md)
+ Advanced: [Walkthrough: Simplify AMI patching using Automation, AWS Lambda, and Parameter Store](automation-walk-patch-windows-ami-simplify.md)
+ Advanced: [Walkthrough: Patch an AMI and update an Auto Scaling group](automation-walk-patch-windows-ami-autoscaling.md)
+ Advanced: [Walkthrough: Run the EC2Rescue tool on unreachable instances](automation-ec2rescue.md)
+ Advanced: [Walkthrough: Reset passwords and SSH keys on EC2 instances](automation-ec2reset.md)
+ Advanced: [Walkthrough: Using Automation with Jenkins](automation-jenkins.md)

**Change Calendar**  
[AWS Systems Manager Change Calendar](systems-manager-change-calendar.md) lets you set up date and time ranges when actions you specify are or not permitted to occur in your AWS account\. 
+ [Create a Change Calendar entry](change-calendar-create.md)
+ [Create a Change Calendar event](change-calendar-create-event.md)

**Maintenance Windows**  
The [AWS Systems Manager Maintenance Windows](systems-manager-maintenance.md) capability lets you define a schedule for performing potentially disruptive actions on your managed instances, such as patching an operating system, updating drivers, or installing software or patches\.
+ [Tutorial: Create and configure a maintenance window \(AWS CLI\)](maintenance-windows-cli-tutorials-create.md)
+ [Tutorial: Update a maintenance window \(AWS CLI\)](maintenance-windows-cli-tutorials-update.md)
+ [Tutorial: View information about maintenance windows \(AWS CLI\)](maintenance-windows-cli-tutorials-describe.md)
+ [Tutorial: View information about tasks and task executions \(AWS CLI\)](mw-cli-tutorial-task-info.md)

## Node Management<a name="getting-started-walkthroughs-node-management"></a>

**Fleet Manager**  
[AWS Systems Manager Fleet Manager](fleet.md) is a unified user interface \(UI\) experience that helps you remotely manage your server fleet running on AWS, or on premises\. With Fleet Manager, you can view the health and performance status of your entire server fleet from one console\. 
+ [Viewing the file system](fleet-file-management.md)

**Compliance**  
The [AWS Systems Manager Compliance](systems-manager-compliance.md) capability scans your fleet of managed instances for patch compliance and configuration inconsistencies\.
+ [Compliance walkthrough \(AWS CLI\)](sysman-compliance-walk.md)

**Inventory**  
The [AWS Systems Manager Inventory](systems-manager-inventory.md) capability collects information about your instances and the software installed on them, helping you to understand your system configurations and installed applications\.
+ Advanced: [Walkthrough: Assign custom inventory metadata to an instance](sysman-inventory-walk-custom.md)
+ Advanced: [Walkthrough: Configure your managed instances for Inventory by using the CLI](sysman-inventory-cliwalk.md)
+ Advanced: [Walkthrough: Use Resource Data Sync to aggregate inventory data](sysman-inventory-resource-data-sync.md)

**Session Manager**  
The [AWS Systems Manager Session Manager](session-manager.md) capability lets you manage your EC2 instances through an interactive one\-click browser\-based shell or through the AWS CLI without the need to open inbound ports, maintain bastion hosts, or manage SSH keys\.
+ [Working with Session Manager](session-manager-working-with.md)

**Run Command**  
The [AWS Systems Manager Run Command](execute-remote-commands.md) capability provides you safe, secure remote management of your instances at scale without logging into your servers, replacing the need for bastion hosts, SSH, or remote PowerShell\. It provides a simple way of automating common administrative tasks across groups of instances such as registry edits, user management, and software and patch installations\.
+ [Walkthrough: Use the AWS CLI with Run Command](walkthrough-cli.md)
+ [Walkthrough: Use the AWS Tools for Windows PowerShell with Run Command](walkthrough-powershell.md)

**State Manager**  
The [AWS Systems Manager State Manager](systems-manager-state.md) capability helps you maintain consistent configuration of your EC2 instances or on\-premises servers and VMs, in a state that you define\. Using State Manager, you can control configuration details such as server configurations, anti\-virus definitions, firewall settings, and more\.
+ [Walkthrough: Creating associations that run MOF files](systems-manager-state-manager-using-mof-file.md)
+ [Walkthrough: Automatically update SSM Agent \(CLI\)](sysman-state-cli.md)
+ [Walkthrough: Automatically update PV drivers on EC2 instances for Windows Server \(console\)](sysman-state-pvdriver.md)

**Patch Manager**  
The [AWS Systems Manager Patch Manager](systems-manager-patch.md) capability helps you select and deploy operating system and software patches automatically across large groups of EC2 instances or on\-premises servers and VMs\.
+ [Working with custom patch baselines](sysman-patch-baseline-console.md)
+ [Creating a patch group](sysman-patch-group-tagging.md)
+ [Walkthrough: Patch a server environment \(AWS CLI\)](sysman-patch-cliwalk.md)

**Distributor**  
The [AWS Systems Manager Distributor](distributor.md) capability lets you package your own software—or find AWS\-provided agent software packages, such as AmazonCloudWatchAgent—to install on Systems Manager managed instances\.
+ [Create a package](distributor-working-with-packages-create.md)
+ [Add a package to Distributor](distributor-working-with-packages-create.md#distributor-working-with-packages-add)

## Shared Resources<a name="getting-started-walkthroughs-shared-resources"></a>

**Documents**  
The [AWS Systems Manager documents](sysman-ssm-docs.md) capability lets you create and manage *SSM documents*\. An SSM document defines the actions that Systems Manager performs on your managed instances\. Systems Manager includes more than a dozen pre\-configured documents that you can use by specifying parameters at runtime\. Documents use JavaScript Object Notation \(JSON\) or YAML, and they include steps and parameters that you specify\. 
+ [Create an SSM document \(console\)](create-ssm-console.md)
+ [Create an SSM document \(command line\)](create-ssm-document-cli.md)