# Systems Manager Automation documents reference<a name="automation-documents-reference"></a>

To help you get started quickly, Systems Manager provides pre\-defined Automation documents, or playbooks\. These documents are maintained by Amazon Web Services and AWS Support\. The Actions Reference describes the actions \(or plugins\) that you can specify in an Automation document\. The Automation Document Details Reference describes each of the predefined Automation documents provided by AWS Systems Manager and AWS Support\.

**Important**  
If you run an automation workflow that invokes other services by using an AWS Identity and Access Management \(IAM\) service role, be aware that the service role must be configured with permission to invoke those services\. This requirement applies to all AWS Automation documents \(`AWS-*` documents\) such as the `AWS-ConfigureS3BucketLogging`, `AWS-CreateDynamoDBBackup`, and `AWS-RestartEC2Instance` documents, to name a few\. This requirement also applies to any custom Automation documents you create that invoke other AWS services by using actions that call other services\. For example, if you use the `aws:executeAwsApi`, `aws:createStack`, or `aws:copyImage` actions, then you must configure the service role with permission to invoke those services\. You can enable permissions to other AWS services by adding an IAM inline policy to the role\. For more information, see [\(Optional\) add an Automation inline policy to invoke other AWS services](automation-permissions.md#automation-role-add-inline-policy)\.

**Topics**
+ [Automation document schema and syntax](automation-doc-syntax.md)
+ [Automation system variables](automation-variables.md)
+ [Systems Manager Automation document details reference](automation-documents-reference-details.md)