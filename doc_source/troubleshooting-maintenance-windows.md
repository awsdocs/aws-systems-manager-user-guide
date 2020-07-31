# Troubleshooting maintenance windows<a name="troubleshooting-maintenance-windows"></a>

Use the following information to help you troubleshoot problems with maintenance windows\.

**Topics**
+ [Edit task error: On the page for editing a maintenance window task, the IAM role list returns an error message: "We couldn't find the IAM maintenance window role specified for this task\. It might have been deleted, or it might not have been created yet\."](#maintenance-window-role-troubleshooting)
+ [Not all maintenance window targets are updated](#targets-not-updated)

## Edit task error: On the page for editing a maintenance window task, the IAM role list returns an error message: "We couldn't find the IAM maintenance window role specified for this task\. It might have been deleted, or it might not have been created yet\."<a name="maintenance-window-role-troubleshooting"></a>

**Problem 1**: The IAM maintenance window role you originally specified was deleted after you created the task\.

**Possible fixes**: \(1\) Select a different IAM maintenance window role, if one exists in your account, or create a new one and select it for the task\. \(2\) Create or select a Systems Manager service\-linked role\. For more information, see [Should I use a service\-linked role or a custom service role to run maintenance window tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)\.

**Problem 2**: If the task was created using the AWS CLI, Tools for Windows PowerShell, or an AWS SDK, a non\-existent IAM maintenance window role name could have been specified\. For example, the IAM maintenance window role could have been deleted before you created the task, or the role name could have been typed incorrectly, such as **myrole** instead of **my\-role**\.

**Possible fixes**: \(1\) Select the correct name of the IAM maintenance window role you want to use, or create a new one to specify for the task\. \(2\) Create or select a Systems Manager service\-linked role\. For more information, see [Should I use a service\-linked role or a custom service role to run maintenance window tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)\.

## Not all maintenance window targets are updated<a name="targets-not-updated"></a>

**Problem:** You notice that maintenance window tasks didn't run on all the resources targeted by your maintenance window\. For example, in the maintenance window run results, the task for that resource is marked as failed or timed out\.
+ **Solution:** The most common reasons for a maintenance window task not running on a target resource involve connectivity and availability\. For example:
  + Systems Manager lost connection to the resource before or during the maintenance window operation\.
  + The resource was offline or stopped during the maintenance window operation\.

You can wait for the next scheduled maintenance window time to run tasks on the resources\. You can manually run the maintenance window tasks on the resources that weren't available or were offline\.