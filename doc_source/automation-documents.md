# Creating your own runbooks<a name="automation-documents"></a>

An Automation runbook defines the *actions* that Systems Manager performs on your managed instances and other AWS resources when an automation runs\. Automation is a capability of AWS Systems Manager\. A runbook contains one or more steps that run in sequential order\. Each step is built around a single action\. Output from one step can be used as input in a later step\. 

The process of running these actions and their steps is called the *automation*\.

Action types supported for runbooks let you automate a wide variety of operations in your AWS environment\. For example, using the `executeScript` action type, you can embed a python or PowerShell script directly in your runbook\. \(When you create a custom runbook, you can add your script inline, or attach it from an S3 bucket or from your local machine\.\) You can automate management of your AWS CloudFormation resources by using the `createStack` and `deleteStack` action types\. In addition, using the `executeAwsApi` action type, a step can run *any *API operation in any AWS service, including creating or deleting AWS resources, starting other processes, initiating notifications, and many more\. 

For a list of all 20 supported action types for Automation, see [Systems Manager Automation actions reference](automation-actions.md)\.

AWS Systems Manager Automation provides several runbooks with pre\-defined steps that you can use to perform common tasks like restarting one or more Amazon Elastic Compute Cloud \(Amazon EC2\) instances or creating an Amazon Machine Image \(AMI\)\. You can also create your own runbooks and share them with other AWS accounts, or make them public for all Automation users\.

Runbooks are written using YAML or JSON\. Using the **Document Builder** in the Systems Manager Automation console, however, you can create a runbook without having to author in native JSON or YAML\.

**Important**  
If you run an automation workflow that invokes other services by using an AWS Identity and Access Management \(IAM\) service role, be aware that the service role must be configured with permission to invoke those services\. This requirement applies to all AWS Automation runbooks \(`AWS-*` runbooks\) such as the `AWS-ConfigureS3BucketLogging`, `AWS-CreateDynamoDBBackup`, and `AWS-RestartEC2Instance` runbooks, to name a few\. This requirement also applies to any custom Automation runbooks you create that invoke other AWS services by using actions that call other services\. For example, if you use the `aws:executeAwsApi`, `aws:createStack`, or `aws:copyImage` actions, configure the service role with permission to invoke those services\. You can give permissions to other AWS services by adding an IAM inline policy to the role\. For more information, see [\(Optional\) Add an Automation inline policy to invoke other AWS services](automation-setup-iam.md#add-inline-policy)\.

For information about the actions that you can specify in a runbook, see [Systems Manager Automation actions reference](automation-actions.md)\.

For information about using the AWS Toolkit for Visual Studio Code to create runbooks, see [Working with Systems Manager Automation documents](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/systems-manager-automation-docs.html) in the *AWS Toolkit for Visual Studio Code User Guide*\.

For information about using Document Builder to create a custom runbook, see [Using Document Builder to create runbooks](automation-document-builder.md)\. 

**Contents**
+ [Authoring Automation runbooks](automation-authoring-runbooks.md)
  + [Identify your use case](automation-authoring-runbooks.md#automation-authoring-runbooks-use-case)
  + [Set up your development environment](automation-authoring-runbooks.md#automation-authoring-runbooks-environment)
  + [Develop runbook content](automation-authoring-runbooks.md#automation-authoring-runbooks-developing-content)
  + [Example 1: Creating parent\-child runbooks](automation-authoring-runbooks-parent-child-example.md)
    + [Create the child runbook](automation-authoring-runbooks-parent-child-example.md#automation-authoring-runbooks-child-runbook)
    + [Create the parent runbook](automation-authoring-runbooks-parent-child-example.md#automation-authoring-runbooks-parent-runbook)
  + [Example 2: Scripted runbook](automation-authoring-runbooks-scripted-example.md)
  + [Additional runbook examples](automation-document-examples.md)
    + [Deploy VPC architecture and Microsoft Active Directory domain controllers](automation-document-architecture-deployment-example.md)
    + [Restore a root volume from the latest snapshot](automation-document-instance-recovery-example.md)
    + [Create an AMI and cross\-Region copy](automation-document-backup-maintenance-example.md)
+ [Creating input parameters that populate AWS resources](populating-input-parameters.md)
+ [Using Document Builder to create runbooks](automation-document-builder.md)
  + [Create a runbook using Document Builder](automation-document-builder.md#create-runbook)
  + [Create a runbook that runs scripts](automation-document-builder.md#create-runbook-scripts)
+ [Using scripts in runbooks](automation-document-script-considerations.md)
  + [Permissions for using runbooks](automation-document-script-considerations.md#script-permissions)
  + [Adding scripts to runbooks](automation-document-script-considerations.md#adding-scripts)
  + [Script constraints for runbooks](automation-document-script-considerations.md#script-constraints)
+ [Using conditional statements in runbooks](automation-branch-condition.md)
  + [Working with the `aws:branch` action](automation-branch-condition.md#branch-action-explained)
    + [Creating an `aws:branch` step in a runbook](automation-branch-condition.md#create-branch-action)
      + [About creating the output variable](automation-branch-condition.md#branch-action-output)
    + [Example `aws:branch` runbooks](automation-branch-condition.md#branch-runbook-examples)
    + [Creating complex branching automations with operators](automation-branch-condition.md#branch-operators)
  + [Examples of how to use conditional options](automation-branch-condition.md#conditional-examples)
+ [Using action outputs as inputs](automation-action-outputs-inputs.md)
  + [Using JSONPath in runbooks](automation-action-outputs-inputs.md#automation-action-json-path)
+ [Creating webhook integrations for Automation](creating-webhook-integrations.md)
  + [Creating integrations \(console\)](creating-webhook-integrations.md#creating-integrations-console)
  + [Creating integrations \(command line\)](creating-webhook-integrations.md#creating-integrations-commandline)
  + [Creating webhooks for integrations](creating-webhook-integrations.md#creating-webhooks)
+ [Handling timeouts in runbooks](automation-handling-timeouts.md)