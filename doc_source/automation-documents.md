# Working with Automation Documents \(Playbooks\)<a name="automation-documents"></a>

A Systems Manager Automation document, or *operational playbook*, defines the *actions* that Systems Manager performs on your managed instances and other AWS resources when an automation execution runs\. A document contains one or more steps that run in sequential order\. Each step is built around a single action\. Output from one step can be used as input in a later step\. 

The process of running these actions and their steps is called the *automation workflow*\.

Action types supported for Automation documents let you automate a wide variety of operations in your AWS environment\. For example, using the `executeScript` action type, you can embed a python or PowerShell script directly in your workflow\. \(When you create a custom Automation document, you can add your script inline, or attach it from an Amazon S3 bucket or from your local machine\.\) You can automate management of your AWS CloudFormation resources by using the `createStack` and `deleteStack` action types\. In addition, using the `executeAwsApi` action type, a step can run *any *API operation in any AWS service, including creating or deleting AWS resources, starting other processes, triggering notifications, and many more\. 

For a list of all 20 supported action types for Automation steps, see [Systems Manager Automation Actions Reference](automation-actions.md)\.

AWS Systems Manager Automation provides several documents with pre\-defined steps that you can use to perform common tasks like restarting one or more Amazon EC2 instances or creating an Amazon Machine Image \(AMI\)\. You can also create your own Automation documents and share them with other AWS accounts, or make them public for all Automation users\.

Automation documents are written using JavaScript Object Notation \(JSON\) or YAML\. Using the **Document Builder** in the Systems Manager Automation console, however, you can create an Automation document without having to author in native JSON or YAML\.

**Important**  
If you run an automation that invokes other services by using an AWS Identity and Access Management \(IAM\) service role, be aware that the service role must be configured with permission to invoke those services\. This requirement applies to all AWS Automation documents \(`AWS-*` documents\) such as the `AWS-ConfigureS3BucketLogging`, `AWS-CreateDynamoDBBackup`, and `AWS-RestartEC2Instance` documents, to name a few\. This requirement also applies to any custom Automation documents you create that invoke other AWS services by using actions that call other services\. For example, if you use the `aws:executeAwsApi`, `aws:createStack`, or `aws:copyImage` actions, then you must configure the service role with permission to invoke those services\. You can enable permissions to other AWS services by adding an IAM inline policy to the role\. For more information, see [\(Optional\) Add an Automation Inline Policy to Invoke Other AWS Services](automation-permissions.md#automation-role-add-inline-policy)\.

For information about the actions or plugins that you can specify in a Systems Manager Automation document, see [Systems Manager Automation Actions Reference](automation-actions.md)\.

For information about the AWS managed Automation documents that run scripts, see [Amazon Managed Automation Documents that Run Scripts](runbook-scripts.md)\.

For information about using Document Builder to create a custom Automation document, see [Creating Automation Documents Using Document Builder](automation-document-builder.md)\. 

For information about creating custom Automation documents that run scripts, see the following topics:
+ [Creating Automation Documents That Run Scripts](automation-document-script.md) – Provides information for using Document Builder to create an Automation document that includes the `aws:executeScript` action\.
+ [Creating an Automation Document that Runs Scripts \(Command Line\)](automation-document-script-commandline.md) – Provides information for using a command line tool to create an Automation document that runs a script\.
+ [ Walkthrough: Using Document Builder to Create a Custom Automation Document](automation-walk-document-builder.md) – Provides step\-by\-step guidance for creating an Automation document that runs scripts to \(1\) launch an Amazon EC2 instance and \(2\) wait for the instance status to change to `ok`\.

**Topics**
+ [Creating Automation Documents Using Document Builder](automation-document-builder.md)
+ [Creating an Automation Document Using the Editor](automation-document-editor.md)
+ [Creating Automation Documents That Run Scripts](automation-document-script.md)
+ [Creating Dynamic Automation Workflows with Conditional Branching](automation-branchdocs.md)
+ [Invoking Other AWS Services from a Systems Manager Automation Workflow](automation-aws-apis-calling.md)
+ [Systems Manager Automation Actions Reference](automation-actions.md)
+ [Systems Manager Automation Documents Reference](automation-documents-reference.md)
+ [Custom Automation Document Samples](automation-document-samples.md)