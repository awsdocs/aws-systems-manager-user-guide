# Creating runbooks that run scripts<a name="automation-document-script"></a>

Automation runbooks support running scripts as part of the automation\. Automation is a capability of AWS Systems Manager\. By using runbooks, you can run scripts directly in AWS without creating a separate compute environment to run your scripts\. Because runbooks can run script steps along with other automation step types, such as approvals, you can manually intervene in critical or ambiguous situations\. 

## Permissions for using runbooks<a name="execution-permissions"></a>

To use a runbook, Systems Manager must use the permissions of an AWS Identity and Access Management \(IAM\) role\. The method that Automation uses to determine which role's permissions to use depends on a few factors, and whether a step uses the `aws:executeScript` action\. 

For runbooks that do not use `aws:executeScript`, Automation uses one of two sources of permissions:
+ The permissions of an IAM service role, or Assume role, that is specified in the runbook or passed in as a parameter\.
+ If no IAM service role is specified, the permissions of the IAM user who started the automation\. 

When a step in a runbook includes the `aws:executeScript` action, however, an IAM service role \(Assume role\) is always required if the Python or PowerShell script specified for the action is calling any AWS API actions\. Automation checks for this role in the following order:
+ The permissions of an IAM service role, or Assume role, that is specified in the runbook or passed in as a parameter\.
+ If no role is found, Automation attempts to run the Python or PowerShell script specified for `aws:executeScript` without any permissions\. If the script is calling an AWS API operation \(for example the Amazon EC2 `CreateImage` operation\), or attempting to act on an AWS resource \(such as an EC2 instance\), the step containing the script fails, and Systems Manager returns an error message reporting the failure\. 

For more information about how to use a runbook that uses an IAM service role or more advanced forms of delegated administration instead, see [Running an automation by using an IAM service role](automation-walk-security-assume.md)\.

## Adding scripts to runbooks<a name="adding-scripts"></a>

You can add scripts to your runbooks by including the script inline as part of a step in the runbook\. You can also attach scripts to the runbook by uploading the scripts from your local machine or by specifying an Amazon Simple Storage Service \(Amazon S3\) bucket where the scripts are located\. After a step that runs a script completes, the output of the script is available as a JSON object, which you can then use as input for subsequent steps in your runbook\.

## Script constraints for runbooks<a name="script-constraints"></a>

The automation action `aws:executeScript` currently supports running Python 3\.6, Python 3\.7, and PowerShell Core 6\.0 scripts\.

Runbooks enforce a limit of five file attachments\. Scripts can either be in the form of a Python script \(\.py\), a PowerShell Core script \(\.ps1\), or attached as contents within a \.zip file\.

Your account is charged for running scripts using Automation\. Automation steps that use the action `aws:executeScript` are considered *special steps*\. There is no step limit for special steps, but your account is charged based on the number of steps and duration of your script execution\. For more information, see the [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/) page\.

The following topics describe how to create runbooks that run scripts\.

**Topics**
+ [Permissions for using runbooks](#execution-permissions)
+ [Adding scripts to runbooks](#adding-scripts)
+ [Script constraints for runbooks](#script-constraints)
+ [Creating a runbook that runs a script \(console\)](automation-document-script-console.md)
+ [Creating a runbook that runs scripts \(command line\)](automation-document-script-commandline.md)
+ [AWS managed runbooks that run scripts](runbook-scripts.md)