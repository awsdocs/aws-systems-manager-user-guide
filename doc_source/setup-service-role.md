# Step 7: \(Optional\) Create Systems Manager service roles<a name="setup-service-role"></a>

This topic explains the difference between a *service role* and a *service\-linked role* for Systems Manager\. It also explains when you need to create or use either type of role\.

**Service role**: A service role is an AWS Identity and Access Management \(IAM\) that grants permissions to an AWS service so that the service can access AWS resources\. Only a few Systems Manager scenarios require a service role\. When you create a service role for Systems Manager, you choose the permissions to grant in order for it to access or interact with other AWS resources\.

**Service\-linked role**: A service\-linked role is predefined by Systems Manager and includes all the permissions that the service requires to call other AWS services on your behalf\.

Currently, the Systems Manager service\-linked role can be used for the following:
+ The Systems Manager Inventory capability uses the service\-linked role to collect inventory metadata from tags and resource groups\.
+ The Maintenance Windows capability can use the service\-linked role in some situations\. Other situations require a custom service role that you create, as described below\.

For more information about the service\-linked role, see [Using service\-linked roles for Systems Manager](using-service-linked-roles.md)\.

## Create a service role<a name="setup-service-role-create"></a>

You can create the following service roles as part of Systems Manager setup, or you can create them later\.

### Service role for Automation<a name="setup-service-role-create-automation"></a>

Automation previously required that you specify a service role so that the service had permission to perform actions on your behalf\. Automation no longer requires this role because the service now operates by using the context of the user who invoked the execution\. 

However, the following situations still require that you specify a service role for Automation:
+ When you want to restrict a user's privileges on a resource, but you want the user to run an Automation workflow that requires elevated privileges\. In this scenario, you can create a service role with elevated privileges and allow the user to run the workflow\.
+ Operations that you expect to run longer than 12 hours require a service role\.

If you need to create a service role and an instance profile role for Automation, you can use one of the following methods\.
+ [Method 1: Use AWS CloudFormation to configure a service role for Automation](automation-cf.md)
+ [Method 2: Use IAM to configure roles for Automation](automation-permissions.md)

### Service role for maintenance window tasks<a name="setup-service-role-create-mw-tasks"></a>

To run tasks on your managed instances, the Maintenance Windows service must have permission to access those resources\. This permission can be granted using either a service\-linked role for Systems Manager or a custom service role that you create\.

You create a custom service role in the following cases: 
+ If you want to use a more restrictive set of permissions than those provided by the service\-linked role\.
+ If you need a more permissive or expanded set of permissions than those provided by the service\-linked role\. For example, some actions in Automation documents require permissions for actions in other AWS services\.

For more information, see the following topics in the Maintenance Windows section of this user guide:
+  [Should I use a service\-linked role or a custom service role to run maintenance window tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role) 
+  [\(Optional\) Create a custom service role for Maintenance Windows \(Console\)](sysman-maintenance-perm-console.md#sysman-maintenance-role)\.

### Service role for Amazon Simple Notification Service notifications<a name="setup-service-role-create-sns"></a>

Amazon Simple Notification Service \(Amazon SNS\) is a web service that coordinates and manages the delivery or sending of messages to subscribing endpoints or clients\. In Systems Manager, you can configure Amazon SNS to send notifications about the status of commands that you send using the Run Command capability, or the status of tasks run in maintenance windows\.

You create a service role for Amazon SNS as part of the process of configuring the service for use with Systems Manager\. After you complete this configuration, you choose whether to receive notifications for particular Run Command commands or maintenance windows tasks at the time you create each one\. 

For more information, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\. 

### Service role for a Systems Manager hybrid environment<a name="setup-service-role-hybrid-environment"></a>

If you plan to use Systems Manager to manage on\-premises servers and virtual machines \(VMs\) in what is called a *hybrid environment*, you must create an IAM role for those resources to communicate with the Systems Manager service\.

For more information, see [Create an IAM service role for a hybrid environment](sysman-service-role.md)\. 

Continue to [Step 8: \(Optional\) Set up integrations with other AWS services](setup-integrations.md)\.