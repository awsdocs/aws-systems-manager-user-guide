# Step 3: Try Systems Manager tutorials and walkthroughs<a name="getting-started-walkthroughs"></a>

This topic guides you to tutorials, walkthroughs, and basic tasks to help you learn how to use AWS Systems Manager\.

Because Systems Manager is a collection of multiple capabilities, no single walkthrough or tutorial can introduce the entire service\. Therefore, we've provided links to resources according to the capability for which they provide practice\.

Usually, you don't need to complete additional setup or configuration tasks before you start\. You can complete all of the tasks if you have necessary permissions and, where needed, access to one or more managed instances\.

Sometimes, additional configuration, setup, or experience with Systems Manager are required before you try a tutorial or walkthrough\. We've identified those tutorials and walkthroughs as "Advanced"\.

**Topics**
+ [Operations management](#getting-started-walkthroughs-operations-management)
+ [Application management](#getting-started-walkthroughs-application-management)
+ [Change management](#getting-started-walkthroughs-change-management)
+ [Node management](#getting-started-walkthroughs-node-management)
+ [Shared resources](#getting-started-walkthroughs-shared-resources)

## Operations management<a name="getting-started-walkthroughs-operations-management"></a>

**Explorer**  
[Explorer](Explorer.md), a capability of AWS Systems Manager, is a customizable operations dashboard that reports information about your AWS resources\. Explorer displays an aggregated view of operations data \(OpsData\) for your AWS accounts and across AWS Regions\. 
+ [Customizing the display and using filters](Explorer-using-filters.md)

**OpsCenter**  
[OpsCenter](OpsCenter.md), a capability of AWS Systems Manager, provides a central location where operations engineers and IT professionals can view, investigate, and resolve operational work items \(OpsItems\) related to AWS resources\.
+ [Configuring CloudWatch to create OpsItems from alarms](OpsCenter-create-OpsItems-from-CloudWatch-Alarms.md)
+ [Configuring EventBridge to automatically create OpsItems for specific events](OpsCenter-automatically-create-OpsItems-2.md)

## Application management<a name="getting-started-walkthroughs-application-management"></a>

**Application Manager**  
[Application Manager](application-manager.md), a capability of AWS Systems Manager, helps DevOps engineers investigate and remediate issues with their AWS resources in the context of their applications\. Application Manager aggregates operations information from multiple AWS services and Systems Manager capabilities to a single AWS Management Console\. 
+ [Viewing overview information about an application](application-manager-working-viewing-overview.md)

**Parameter Store**  
[Parameter Store](systems-manager-parameter-store.md), a capability of AWS Systems Manager, provides a centralized store to manage your configuration data, whether plaintext data such as database strings or secrets such as passwords\. This allows you to separate your secrets and configuration data from your code\. You can tag and organize parameters into hierarchies to help manage them\.
+ [Create a Systems Manager parameter \(console\)](parameter-create-console.md)
+ [Create a Systems Manager parameter \(AWS CLI\)](param-create-cli.md)
+ [Working with parameter hierarchies](sysman-paramstore-hierarchies.md)
+ Advanced: [Create a SecureString parameter and join a node to a Domain \(PowerShell\)](sysman-param-securestring-walkthrough.md)

## Change management<a name="getting-started-walkthroughs-change-management"></a>

**Change Manager**  
[Change Manager](change-manager.md), a capability of AWS Systems Manager, is an enterprise change management framework for requesting, approving, implementing, and reporting on operational changes to your application configuration and infrastructure\.
+ Advanced: [Try out the AWS managed `Hello World` change template](change-templates-aws-managed.md)

**Automation**  
[Automation](systems-manager-automation.md), a capability of AWS Systems Manager, allows you to safely automate operations and management tasks across AWS resources\. You can automate common IT tasks, safely perform disruptive tasks in bulk, simplify complex tasks, enhance operations security, and use stored configuration scripts to share best practices with the rest of your organization\.
+ Advanced: [Walkthrough: Patch a Linux AMI \(console\)](automation-walk-patch-linux-ami-console.md)
+ Advanced: [Patch a Linux AMI \(AWS CLI\)](automation-walk-patch-linux-ami-cli.md)
+ Advanced: [Patch a Windows Server AMI](automation-walk-patch-windows-ami-cli.md)
+ Advanced: [Simplify AMI patching using Automation, AWS Lambda, and Parameter Store](automation-walk-patch-windows-ami-simplify.md)
+ Advanced: [Patch an AMI and update an Auto Scaling group](automation-walk-patch-windows-ami-autoscaling.md)
+ Advanced: [Run the EC2Rescue tool on unreachable instances](automation-ec2rescue.md)
+ Advanced: [Reset passwords and SSH keys on EC2 instances](automation-ec2reset.md)
+ Advanced: [Using Automation with Jenkins](automation-jenkins.md)

**Change Calendar**  
[Change Calendar](systems-manager-change-calendar.md), a capability of AWS Systems Manager, helps you set up date and time ranges when actions you specify are or are not permitted to occur in your AWS account\. 
+ [Creating a change calendar](change-calendar-create.md)
+ [Creating a Change Calendar event](change-calendar-create-event.md)

**Maintenance Windows**  
[Maintenance Windows](systems-manager-maintenance.md), a capability of AWS Systems Manager, helps you define a schedule for performing potentially disruptive actions on your managed instances, such as patching an operating system, updating drivers, or installing software or patches\.
+ [Tutorial: Create and configure a maintenance window \(AWS CLI\)](maintenance-windows-cli-tutorials-create.md)
+ [Tutorial: Update a maintenance window \(AWS CLI\)](maintenance-windows-cli-tutorials-update.md)
+ [Tutorial: View information about maintenance windows \(AWS CLI\)](maintenance-windows-cli-tutorials-describe.md)
+ [Tutorial: View information about tasks and task executions \(AWS CLI\)](mw-cli-tutorial-task-info.md)

## Node management<a name="getting-started-walkthroughs-node-management"></a>

**Fleet Manager**  
[Fleet Manager](fleet.md), a capability of AWS Systems Manager, is a unified user interface \(UI\) experience that helps you remotely manage your managed nodes\. With Fleet Manager, you can view the health and performance status of your entire managed\-node fleet from one console\. 
+ [Working with the file system ](fleet-file-management.md)

**Compliance**  
[Compliance](systems-manager-compliance.md), a capability of AWS Systems Manager, scans your fleet of managed nodes for patch compliance and configuration inconsistencies\.
+ [Compliance walkthrough \(AWS CLI\)](sysman-compliance-walk.md)

**Inventory**  
[Inventory](systems-manager-inventory.md), a capability of AWS Systems Manager, collects information about your managed nodes and the software installed on them, helping you to understand your system configurations and installed applications\.
+ Advanced: [Walkthrough: Assign custom inventory metadata to a managed node](sysman-inventory-walk-custom.md)
+ Advanced: [Walkthrough: Configure your managed nodes for Inventory by using the CLI](sysman-inventory-cliwalk.md)
+ Advanced: [Walkthrough: Use resource data sync to aggregate inventory data](sysman-inventory-resource-data-sync.md)

**Session Manager**  
[Session Manager](session-manager.md), a capability of AWS Systems Manager, helps you manage your Amazon EC2 instances and AWS IoT Greengrass core devices through an interactive one\-click browser\-based shell or through the AWS CLI without the need to open inbound ports, maintain bastion hosts, or manage SSH keys\.
+ [Working with Session Manager](session-manager-working-with.md)

**Run Command**  
[Run Command](execute-remote-commands.md), a capability of AWS Systems Manager, provides secure remote management of your managed nodes at scale without logging into them\. Run Command helps you manage your nodes without using bastion hosts, SSH, or remote PowerShell\. Run Command also provides a simple way of automating common administrative tasks across groups of managed nodes such as registry edits, user management, and software and patch installations\.
+ [Walkthrough: Use the AWS CLI with Run Command](walkthrough-cli.md)
+ [Walkthrough: Use the AWS Tools for Windows PowerShell with Run Command](walkthrough-powershell.md)

**State Manager**  
[State Manager](systems-manager-state.md), a capability of AWS Systems Manager, helps you maintain your fleet of managed nodes in a consistent configuration and state that you define\. Using State Manager, you can control configuration details such as server configurations, antivirus definitions, firewall settings, and more\.
+ [Walkthrough: Creating associations that run MOF files](systems-manager-state-manager-using-mof-file.md)
+ [Walkthrough: Automatically update SSM Agent \(CLI\)](sysman-state-cli.md)
+ [Walkthrough: Automatically update PV drivers on EC2 instances for Windows Server \(console\)](sysman-state-pvdriver.md)

**Patch Manager**  
[Patch Manager](systems-manager-patch.md), a capability of AWS Systems Manager, helps you select and deploy operating system and software patches automatically across large groups of managed nodes\.
+ [Working with custom patch baselines \(console\)](sysman-patch-baseline-console.md)
+ [Working with patch groups](sysman-patch-group-tagging.md)
+ [Walkthrough: Patch a server environment \(AWS CLI\)](sysman-patch-cliwalk.md)

**Distributor**  
[Distributor](distributor.md), a capability of AWS Systems Manager, helps you package your own software—or find AWS provided agent software packages, such as AmazonCloudWatchAgent—to install on Systems Manager managed nodes\.
+ [Create a package](distributor-working-with-packages-create.md)
+ [Add a package to Distributor](distributor-working-with-packages-create.md#distributor-working-with-packages-add)

## Shared resources<a name="getting-started-walkthroughs-shared-resources"></a>

**Documents**  
[Documents](sysman-ssm-docs.md), a capability of AWS Systems Manager, helps you create and manage *SSM documents*\. An SSM document defines the actions that Systems Manager performs on your managed nodes\. Systems Manager includes more than a dozen preconfigured documents that you can use by specifying parameters at runtime\. Documents use JavaScript Object Notation \(JSON\) or YAML, and they include steps and parameters that you specify\. 
+ [Create an SSM document \(console\)](create-ssm-console.md)
+ [Create an SSM document \(command line\)](create-ssm-document-cli.md)