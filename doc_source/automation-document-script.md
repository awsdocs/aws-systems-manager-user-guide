# Creating Automation documents that run scripts<a name="automation-document-script"></a>

AWS Systems Manager Automation documents support running scripts as part of the automation workflow\. By using Automation documents, you can run scripts directly in AWS without creating a separate compute environment to run your scripts\. Because Automation documents can run script steps along with other automation step types, such as approvals, you can manually intervene in critical or ambiguous situations\. 

## Permissions for running Automation executions<a name="execution-permissions"></a>

To run an automation execution, Systems Manager must use the permissions of an AWS Identity and Access Management \(IAM\) role\. The method that Automation uses to determine which role's permissions to use depends on a few factors, and whether a step uses the `aws:executeScript` action\. 

For automation executions that do not use `aws:executeScript`, Automation uses one of two sources of permissions:
+ The permissions of an IAM service role, or Assume role, that is specified in the Automation document or passed in as a parameter\.
+ If no IAM service role is specified, the permissions of the IAM user who started the automation execution\. 

When a step in an Automation document includes the `aws:executeScript` action, however, an IAM service role \(Assume role\) is always required if the Python or PowerShell script specified for the action is calling any AWS API actions\. Automation checks for this role in the following order:
+ The permissions of an IAM service role, or Assume role, that is specified in the Automation document or passed in as a parameter\.
+ A resource tag applied to the Automation document with the tag key `AutomationScriptExecutionRole`\. In this case, Automation uses the IAM role that is specified as the tag value\. For example, `arn:aws:iam::123456789012:role/AutomationAssumeRole`\.

  If no role is found, Automation attempts to run the Python or PowerShell script specified for `aws:executeScript` without any permissions\. If the script is calling an AWS API operation \(for example the Amazon EC2 `CreateImage` operation\), or attempting to act on an AWS resource \(such as an EC2 instance\), the step containing the script fails, and Systems Manager returns an error message reporting the failure\. 

For more information about how to run an Automation workflow that uses an IAM service role or more advanced forms of delegated administration instead, see [Running an automation by using an IAM service role](automation-walk-security-assume.md)\.

## Adding scripts to Automation documents<a name="adding-scripts"></a>

You can add scripts to your Automation documents by including the script inline as part of a step in the document\. You can also attach scripts to the document by uploading the scripts from your local machine or by specifying an Amazon Simple Storage Service \(Amazon S3\) bucket where the scripts are located\. After a step that runs a script completes, the output of the script is available as a JSON object, which you can then use as input for subsequent steps in your Automation workflow\.

## Script constraints for Automation documents<a name="script-constraints"></a>

The Automation action `aws:executeScript` currently supports running Python 3\.6, Python 3\.7, and PowerShell Core 6\.0 scripts\.

Automation documents enforce a limit of five file attachments\. Scripts can either be in the form of a Python script \(\.py\), a PowerShell Core script \(\.ps1\), or attached as contents within a \.zip file\.

Your account is charged for running scripts using Automation\. Automation steps that use the action `aws:executeScript` are considered *special steps*\. There is no step limit for special steps, but your account is charged based on the number of steps and duration of your script execution\. For more information, see the [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/) page\.

The following topics describe how to create Automation documents that run scripts\.

**Topics**
+ [Permissions for running Automation executions](#execution-permissions)
+ [Adding scripts to Automation documents](#adding-scripts)
+ [Script constraints for Automation documents](#script-constraints)
+ [Creating an Automation document that runs a script \(console\)](automation-document-script-console.md)
+ [Creating an Automation document that runs scripts \(command line\)](automation-document-script-commandline.md)
+ [Amazon managed Automation documents that run scripts](runbook-scripts.md)