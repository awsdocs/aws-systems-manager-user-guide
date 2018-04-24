# AWS Systems Manager Documents<a name="sysman-ssm-docs"></a>

An AWS Systems Manager document \(SSM document\) defines the actions that Systems Manager performs on your managed instances\. Systems Manager includes more than a dozen pre\-configured documents that you can use by specifying parameters at runtime\. Documents use JavaScript Object Notation \(JSON\) or YAML, and they include steps and parameters that you specify\.

**Types of SSM Documents**  
The following table describes the different types of SSM documents\.


****  

| Type | Use with | Details | 
| --- | --- | --- | 
|  Command document  |  [Run Command](execute-remote-commands.md) [State Manager](systems-manager-state.md)  |  Run Command uses command documents to execute commands\. State Manager uses command documents to apply a configuration\. These actions can be run on one or more targets at any point during the lifecycle of an instance,  | 
|  Policy document  |  [State Manager](systems-manager-state.md)  |  Policy documents enforce a policy on your targets\. If the policy document is removed, the policy action \(for example, collecting inventory\) no longer happens\.  | 
|  Automation document  |  [Automation](systems-manager-automation.md)  |  Use automation documents when performing common maintenance and deployment tasks such as creating or updating an Amazon Machine Image \(AMI\)\.  | 

**SSM Document Versions and Execution**  
You can create and save different versions of documents\. You can then specify a default version for each document\. The default version of a document can be updated to a newer version or reverted to an older version of the document\. When you change the content of a document, Systems Manager automatically increments the version of the document\. You can retrieve and use previous versions of a document\.

**Customizing a Document**  
If you want to customize the steps and actions in a document, you can create your own\. The first time you use a document to perform an action on an instance, the system stores the document with your AWS account\. For more information about how to create a Systems Manager document, see [Creating Systems Manager Documents](create-ssm-doc.md)\.

**Tagging a Document**  
You can tag your documents to help you quickly identify one or more documents based on the tags you've assigned to them\. For example, you can tag documents for specific environments, departments, users, groups, or periods\. You can also restrict access to documents by creating an IAM policy that specifies the tags that a user or group can access\. For more information, see [Tagging Systems Manager Documents](sysman-ssm-docs-tagging.md)\.

**Sharing a Document**  
You can make your documents public or share them with specific AWS accounts\. For more information, see [Sharing Systems Manager Documents](ssm-sharing.md)\.

**SSM Document Limits**  
For information about SSM document limits, see [AWS Systems Manager Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_ssm)\.

**Topics**
+ [Systems Manager Pre\-Defined Documents](predefined-documents.md)
+ [SSM Document Schemas and Features](document-schemas-features.md)
+ [SSM Document Syntax](sysman-doc-syntax.md)
+ [Creating Systems Manager Documents](create-ssm-doc.md)
+ [Tagging Systems Manager Documents](sysman-ssm-docs-tagging.md)
+ [Sharing Systems Manager Documents](ssm-sharing.md)
+ [Creating Composite Documents](composite-docs.md)
+ [Running Documents from Remote Locations](run-remote-documents.md)
+ [SSM Document Plugin Reference](ssm-plugins.md)
+ [Systems Manager Automation Document Reference](automation-actions.md)