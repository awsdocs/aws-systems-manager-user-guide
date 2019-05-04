# Systems Manager Maintenance Window Tutorials \(AWS CLI\)<a name="maintenance-windows-tutorials"></a>

This section includes several tutorials to help you learn how to use the AWS CLI to create, configure, update, view information about, and delete Maintenance Windows\. 

## Verify or Complete Tutorial Prerequisites<a name="mw-cli-tutorial-setup"></a>

Before trying these tutorials, complete the following tasks:

**Task 1: Download and configure the AWS CLI**  
For information, see [Installing the AWS Command Line Interface](url-cli-ug;installing.html) and [Configuring the AWS CLI](url-cli-ug;cli-chap-getting-started.html)\.

**Task 2: Configure Maintenance Window roles and permissions**  
For information, see [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)\.

**Task 3: Create or configure Systems Manager\-compatible instances**  
You need at least one properly configured Amazon EC2 instance to complete the tutorials\. We recommend launching an Amazon Linux or Amazon Linux 2 instance, which come with SSM Agent preinstalled\. For information about configuring instances to use with Systems Manager, see the following topics in the **Setting Up** section of this user guide:
+ [Task 2: Create an Instance Profile for Systems Manager](sysman-configuring-access-role.md)
+ [Task 3: Create an Amazon EC2 Instance that Uses the Systems Manager Instance Profile](sysman-create-instance-with-role.md)

**Task 4: Create additional resources as needed**  
Many Run Command type tasks do not require you to create resources other than those listed in this prerequisites topic\. For that reason, we provide a simple Run Command task for you to use your first time through the tutorials\. If a Maintenance Window task you want to run requires additional resources, however, you should create them first\. For example, if you want a maintenance window that runs an AWS Lambda function, create the Lambda function before you begin\. \.

**Topics**
+ [Verify or Complete Tutorial Prerequisites](#mw-cli-tutorial-setup)
+ [Tutorial: Create and Configure a Maintenance Window \(AWS CLI\)](maintenance-windows-cli-tutorials-create.md)
+ [Tutorial: View Information About Tasks and Task Executions \(AWS CLI\)](mw-cli-tutorial-task-info.md)
+ [Tutorial: Update a Maintenance Window \(AWS CLI\)](maintenance-windows-cli-tutorials-update.md)
+ [Tutorial: View Information About Maintenance Windows \(AWS CLI\)](maintenance-windows-cli-tutorials-describe.md)
+ [Tutorial: Delete a Maintenance Window \(AWS CLI\)](mw-cli-tutorial-delete-mw.md)