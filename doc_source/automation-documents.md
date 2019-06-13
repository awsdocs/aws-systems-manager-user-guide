# Working with Automation Documents<a name="automation-documents"></a>

A Systems Manager Automation document defines the actions that Systems Manager performs on your managed instances and AWS resources\. Documents use JavaScript Object Notation \(JSON\) or YAML, and they include steps and parameters that you specify\. Steps run in sequential order\.

Automation documents are Systems Manager documents of type `Automation`, as opposed to `Command` and `Policy` documents\. Automation documents currently support schema version 0\.3\. Command documents use schema version 1\.2, 2\.0, or 2\.2\. Policy documents use schema version 2\.0 or later\.

To view information about the actions or plugins that you can specify in a Systems Manager Automation document, see [Systems Manager Automation Actions Reference](automation-actions.md)\. To view information about the plugins for all other SSM documents, see [SSM Document Plugin Reference](ssm-plugins.md)\.

**Important**  
If you run an automation that invokes other services by using an AWS Identity and Access Management \(IAM\) service role, be aware that the service role must be configured with permission to invoke those services\. This requirement applies to all AWS Automation documents \(`AWS-*` documents\) such as the `AWS-ConfigureS3BucketLogging`, `AWS-CreateDynamoDBBackup`, and `AWS-RestartEC2Instance` documents, to name a few\. This requirement also applies to any custom Automation documents you create that invoke other AWS services by using actions that call other services\. For example, if you use the `aws:executeAwsApi`, `aws:CreateStack`, or `aws:copyImage actions`, to name a few, then you must configure the service role with permission to invoke those services\. You can enable permissions to other AWS services by adding an IAM inline policy to the role\. For more information, see [\(Optional\) Add an Automation Inline Policy to Invoke Other AWS Services](automation-permissions.md#automation-role-add-inline-policy)\.

**Topics**
+ [Creating Dynamic Automation Workflows with Conditional Branching](automation-branchdocs.md)
+ [Invoking Other AWS Services from a Systems Manager Automation Workflow](automation-aws-apis-calling.md)
+ [Sharing a Systems Manager Automation Document](automation-share-document.md)
+ [Systems Manager Automation Documents Reference](automation-documents-reference.md)