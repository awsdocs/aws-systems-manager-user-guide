# Systems Manager Maintenance Window Tutorials \(AWS CLI\)<a name="maintenance-windows-tutorials"></a>

This section includes several tutorials to help you learn how to use the AWS CLI to create, configure, update, view information about, and delete Maintenance Windows\. 

## Before You Begin: Verify or Complete Setup Prerequisites<a name="mw-cli-tutorial-setup"></a>

Before attempting these tutorials, make sure you have completed the following tasks:

**Task 1: Download and configure the AWS CLI**  
For information, see [Installing the AWS Command Line Interface](url-cli-ug;installing.html) and [Configuring the AWS CLI](url-cli-ug;cli-chap-getting-started.html)\.

**Task 2: Configure Maintenance Window roles and permissions**  
For information, see [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)\.

**Task 3: Create or configure Systems Manager\-compatible instances**  
For information about configuring one or more instances on which to run the Maintenance Windows you create, see [Task 2: Create an Instance Profile for Systems Manager](sysman-configuring-access-role.md) and [Task 3: Create an Amazon EC2 Instance that Uses the Systems Manager Instance Profile](sysman-create-instance-with-role.md) in the [Setting Up AWS Systems Manager](systems-manager-setting-up.md) section of this user guide\.

**Task 4: Create additional resources as needed**  
If a Maintenance Window task type you want to run requires additional resources, you should create them first\. For example, if you want to create a Maintenance Window for an AWS Lambda type task, create the Lambda function before you begin\. 

However, many Run Command type tasks do not require you to create any resources beyond those listed in this "Before You Begin" section\. For that reason, we recommend that you create a Maintenance Window for a Run Command type task your first time through the tutorial\.

**Topics**
+ [Before You Begin: Verify or Complete Setup Prerequisites](#mw-cli-tutorial-setup)
+ [Tutorial: Create and Configure a Maintenance Window \(CLI\)](maintenance-windows-cli-tutorials-create.md)
+ [Tutorial: View Information About Tasks and Task Executions \(CLI\)](mw-cli-tutorial-task-info.md)
+ [Tutorial: Update a Maintenance Window \(CLI\)](maintenance-windows-cli-tutorials-update.md)
+ [Tutorial: View Information About Maintenance Windows \(CLI\)](maintenance-windows-cli-tutorials-describe.md)
+ [Tutorial: Delete a Maintenance Window \(CLI\)](mw-cli-tutorial-delete-mw.md)