# Troubleshooting IAM Maintenance Window Permissions<a name="maintenance-window-role-troubleshooting"></a>

Use the following information to help you troubleshoot common issues with Maintenance Windows permissions in AWS Systems Manager\.

## Edit Task Error: On the page for editing a maintenance window task, the IAM role list returns an error message: "We couldn't find the IAM maintenance window role specified for this task\. It might have been deleted, or it might not have been created yet\."<a name="maintenance-window-role-troubleshooting-1"></a>

**Problem 1**: The IAM maintenance window role you originally specified was deleted after you created the task\.

**Possible fixes**: \(1\) Select a different IAM maintenance window role, if one exists in your account, or create a new one and select it for the task\. \(2\) Create or select a Systems Manager service\-linked role\. For more information, see [Should I Use a Service\-Linked Role or a Custom Service Role to Run Maintenance Window Tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)\.

**Problem 2**: If the task was created using the AWS CLI, Tools for Windows PowerShell, or an AWS SDK, a non\-existent IAM maintenance window role name could have been specified\. For example, the IAM maintenance window role could have been deleted before you created the task, or the role name could have been typed incorrectly, such as **myrole** instead of **my\-role**\.

**Possible fixes**: \(1\) Select the correct name of the IAM maintenance window role you want to use, or create a new one to specify for the task\. \(2\) Create or select a Systems Manager service\-linked role\. For more information, see [Should I Use a Service\-Linked Role or a Custom Service Role to Run Maintenance Window Tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)\.