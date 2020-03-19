# Step 3: Try Systems Manager Tutorials and Walkthroughs<a name="getting-started-walkthroughs"></a>

This topic guides you to tutorials, walkthroughs, and basic tasks to help you learn how to use Systems Manager\.

Because Systems Manager is a collection of several capabilities, no single walkthrough or tutorial can introduce the entire service\. Therefore, we've provided links to resources according to the capability for which they provide practice\.

In most cases, you do not need to complete additional setup or configuration tasks before you start\. You can complete all of the tasks if you have necessary permissions and, where needed, access to one or more managed instances\.

In some cases, additional configuration, setup, or experience with Systems Manager are required before you try a tutorial or walkthrough\. We have identify those tutorials and walkthroughs as "Advanced"\.

**Compliance**  
The [AWS Systems Manager Configuration Compliance](systems-manager-compliance.md) capability scans your fleet of managed instances for patch compliance and configuration inconsistencies\.
+ [Configuration Compliance Walkthrough \(AWS CLI\)](sysman-compliance-walk.md)

**Run Command**  
The [AWS Systems Manager Run Command](execute-remote-commands.md) capability provides you safe, secure remote management of your instances at scale without logging into your servers, replacing the need for bastion hosts, SSH, or remote PowerShell\. It provides a simple way of automating common administrative tasks across groups of instances such as registry edits, user management, and software and patch installations\.
+ [Walkthrough: Use the AWS CLI with Run Command](walkthrough-cli.md)
+ [Walkthrough: Use the AWS Tools for Windows PowerShell with Run Command](walkthrough-powershell.md)

**Session Manager**  
The [AWS Systems Manager Session Manager](session-manager.md) capability lets you manage your Amazon EC2 instances through an interactive one\-click browser\-based shell or through the AWS CLI without the need to open inbound ports, maintain bastion hosts, or manage SSH keys\.
+ [Working with Session Manager](session-manager-working-with.md)

**Distributor**  
The [AWS Systems Manager Distributor](distributor.md) capability lets you package your own software—or find AWS\-provided agent software packages, such as AmazonCloudWatchAgent—to install on Systems Manager managed instances\.
+ [Create a Package](distributor-working-with-packages-create.md)
+ [Step 4: Add a Package to Distributor](distributor-working-with-packages-create.md#distributor-working-with-packages-add)

**Patch Manager**  
The [AWS Systems Manager Patch Manager](systems-manager-patch.md) capability helps you select and deploy operating system and software patches automatically across large groups of Amazon EC2 instances or on\-premises servers and VMs\.
+ [Create a Custom Patch Baseline](sysman-patch-baseline-console.md)
+ [Create a Patch Group](sysman-patch-group-tagging.md)
+ [Tutorial: Patch a Server Environment \(Command Line\)](sysman-patch-cliwalk.md)

**Maintenance Windows**  
The [AWS Systems Manager Maintenance Windows](systems-manager-maintenance.md) capability lets you define a schedule for performing potentially disruptive actions on your managed instances, such as patching an operating system, updating drivers, or installing software or patches\.
+ [Tutorial: Create and Configure a Maintenance Window \(AWS CLI\)](maintenance-windows-cli-tutorials-create.md)
+ [Tutorial: Update a Maintenance Window \(AWS CLI\)](maintenance-windows-cli-tutorials-update.md)
+ [Tutorial: View Information About Maintenance Windows \(AWS CLI\)](maintenance-windows-cli-tutorials-describe.md)
+ [Tutorial: View Information About Tasks and Task Executions \(AWS CLI\)](mw-cli-tutorial-task-info.md)

**State Manager**  
The [AWS Systems Manager State Manager](systems-manager-state.md) capability helps you maintain consistent configuration of your Amazon EC2 instances or on\-premises servers and VMs, in a state that you define\. Using State Manager, you can control configuration details such as server configurations, anti\-virus definitions, firewall settings, and more\.
+ [Creating Associations That Run MOF Files](systems-manager-state-manager-using-mof-file.md)
+ [Automatically Update SSM Agent \(CLI\)](sysman-state-cli.md)
+ [Walkthrough: Automatically Update PV Drivers on EC2 Windows Instances \(Console\)](sysman-state-pvdriver.md)

**Documents**  
The [AWS Systems Manager Documents](sysman-ssm-docs.md) capability lets you create and manage *SSM documents*\. An SSM document defines the actions that Systems Manager performs on your managed instances\. Systems Manager includes more than a dozen pre\-configured documents that you can use by specifying parameters at runtime\. Documents use JavaScript Object Notation \(JSON\) or YAML, and they include steps and parameters that you specify\. 
+ [Create an SSM Document \(Console\)](create-ssm-console.md)
+ [Create an SSM Document \(Command Line\)](create-ssm-document-cli.md)

**Parameter Store**  
The [AWS Systems Manager Parameter Store](systems-manager-parameter-store.md) capability provides a centralized store to manage your configuration data, whether plain\-text data such as database strings or secrets such as passwords\. This allows you to separate your secrets and configuration data from your code\. Parameters can be tagged and organized into hierarchies, helping you manage parameters more easily\.
+ [Walkthrough: Create and Test a Parameter \(Console\)](sysman-paramstore-console.md)
+ [Walkthrough: Create and Update a String Parameter \(AWS CLI\)](sysman-paramstore-cli.md)
+ [Walkthrough: Manage Parameters Using Hierarchies \(AWS CLI\)](sysman-paramstore-walk-hierarchies.md)
+ Advanced: [Walkthrough: Create a Secure String Parameter and Join an Instance to a Domain \(PowerShell\)](sysman-param-securestring-walkthrough.md)

**Inventory**  
The [AWS Systems Manager Inventory](systems-manager-inventory.md) capability collects information about your instances and the software installed on them, helping you to understand your system configurations and installed applications\.
+ Advanced: [Walkthrough: Assign Custom Inventory Metadata to an Instance](sysman-inventory-walk-custom.md)
+ Advanced: [Walkthrough: Configure Your Managed Instances for Inventory by Using the CLI](sysman-inventory-cliwalk.md)
+ Advanced: [Walkthrough: Use Resource Data Sync to Aggregate Inventory Data](sysman-inventory-resource-data-sync.md)

**Automation**  
The [AWS Systems Manager Automation](systems-manager-automation.md) capability allows you to safely automate operations and management tasks across AWS resources\. You can automate common IT tasks, safely perform disruptive tasks in bulk, simplify complex tasks, enhance operations security, and used stored configuration scripts share best practices with the rest of your organization\.
+ Advanced: [Walkthrough: Patch a Linux AMI \(Console\)](automation-walk-patch-linux-ami-console.md)
+ Advanced: [Walkthrough: Patch a Linux AMI \(AWS CLI\)](automation-walk-patch-linux-ami-cli.md)
+ Advanced: [Walkthrough: Patch a Windows Server AMI](automation-walk-patch-windows-ami-cli.md)
+ Advanced: [Walkthrough: Simplify AMI Patching Using Automation, AWS Lambda, and Parameter Store](automation-walk-patch-windows-ami-simplify.md)
+ Advanced: [Walkthrough: Patch an AMI and Update an Auto Scaling Group](automation-walk-patch-windows-ami-autoscaling.md)
+ Advanced: [Walkthrough: Run the EC2Rescue Tool on Unreachable Instances](automation-ec2rescue.md)
+ Advanced: [Walkthrough: Reset Passwords and SSH Keys on Amazon EC2 Instances](automation-ec2reset.md)
+ Advanced: [Walkthrough: Using Automation with Jenkins](automation-jenkins.md)