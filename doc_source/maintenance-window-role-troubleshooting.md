# Troubleshooting IAM Maintenance Window Permissions<a name="maintenance-window-role-troubleshooting"></a>

Use the following information to help you troubleshoot common issues with Maintenance Window permissions in AWS Systems Manager\.

## Edit Task Error: On the page for editing a Maintenance Window task, the IAM role list returns an error message: "We couldn't find the IAM Maintenance Window role specified for this task\. It might have been deleted, or it might not have been created yet\."<a name="maintenance-window-role-troubleshooting-1"></a>

**Problem 1**: The IAM Maintenance Window role you originally specified was deleted after you created the task\.

**Possible fixes**: Select a different IAM Maintenance Window role, if one exists in your account, or create a new one and select it for the task\. 

**Problem 2**: If the task was created using the AWS CLI, Tools for Windows PowerShell, or an AWS SDK, a non\-existent IAM Maintenance Window role name could have been specified\. For example, the IAM Maintenance Window role could have been deleted before you created the task, or the role name could have been typed incorrectly, such as **myrole** instead of **my\-role**\.

**Possible fixes**: Select the correct name of the IAM Maintenance Window role you want to use, or create a new one to specify for the task\.