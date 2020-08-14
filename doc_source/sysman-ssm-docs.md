# AWS Systems Manager documents<a name="sysman-ssm-docs"></a>

An AWS Systems Manager document \(SSM document\) defines the actions that Systems Manager performs on your managed instances\. Systems Manager includes more than a dozen pre\-configured documents that you can use by specifying parameters at runtime\. Documents use JavaScript Object Notation \(JSON\) or YAML, and they include steps and parameters that you specify\.

**Types of SSM documents**  
The following table describes the different types of SSM documents\.


****  

| Type | Use with | Details | 
| --- | --- | --- | 
|  Command document  |   [Run Command](execute-remote-commands.md)   [State Manager](systems-manager-state.md)   [Maintenance Windows](systems-manager-maintenance.md)   |  Run Command uses command documents to run commands\. State Manager uses command documents to apply a configuration\. These actions can be run on one or more targets at any point during the lifecycle of an instance\. Maintenance Windows uses command documents to apply a configuration based on the specified schedule\.  | 
|  Automation document  |   [Automation](systems-manager-automation.md)   [State Manager](systems-manager-state.md)   [Maintenance Windows](systems-manager-maintenance.md)   |  Use automation documents when performing common maintenance and deployment tasks such as creating or updating an Amazon Machine Image \(AMI\)\. State Manager uses automation documents to apply a configuration\. These actions can be run on one or more targets at any point during the lifecycle of an instance\. Maintenance Windows uses automation documents to perform common maintenance and deployment tasks based on the specified schedule\.  | 
|  Package document  |   [Distributor](distributor.md)   |  In Distributor, a package is represented by an SSM document\. A package document includes attached ZIP archive files that contain software or assets to install on managed instances\. Creating a package in Distributor creates the package document\.  | 
|  Session document  |   [Session Manager](session-manager.md)   |  Session Manager uses session documents to determine which type of session to start, such as a port forwarding session, a session to run an interactive command, or a session to create an SSH tunnel\.  | 
|  Policy document  |   [State Manager](systems-manager-state.md)   |  Systems Manager Inventory uses the AWS\-GatherSoftwareInventory policy document with a State Manager association to collect inventory data from managed instances\. When creating your own SSM documents, Automation documents and Run Command documents are the preferred method for enforcing a policy on a managed instance\.  | 
|  Change Calendar document  |   [Change Calendar](systems-manager-change-calendar.md)   |  Systems Manager Change Calendar uses the `ChangeCalendar` document type\. A Change Calendar document stores a calendar entry and associated events that can allow or prevent Automation actions from changing your environment\. In Change Calendar, a document stores [iCalendar 2\.0](https://icalendar.org/) data in plain\-text format\.  | 

**SSM document versions and execution**  
You can create and save different versions of documents\. You can then specify a default version for each document\. The default version of a document can be updated to a newer version or reverted to an older version of the document\. When you change the content of a document, Systems Manager automatically increments the version of the document\. You can retrieve or use any version of a document by specifying the document version in the console, CLI commands, or API calls\.

**Customizing a document**  
If you want to customize the steps and actions in a document, you can create your own\. The first time you use a document to perform an action on an instance, the system stores the document with your AWS account\. For more information about how to create an SSM document, see [Creating Systems Manager documents](create-ssm-doc.md)\.

**Tagging a document**  
You can tag your documents to help you quickly identify one or more documents based on the tags you've assigned to them\. For example, you can tag documents for specific environments, departments, users, groups, or periods\. You can also restrict access to documents by creating an IAM policy that specifies the tags that a user or group can access\. For more information, see [Tagging Systems Manager documents](tagging-documents.md)\.

**Sharing a document**  
You can make your documents public or share them with specific AWS accounts in the same AWS Region\. Sharing documents between accounts can be useful if, for example, you want all of the EC2 instances that you supply to customers or employees to have the same configuration\. In addition to keeping applications or patches on the instances up\-to\-date, you might want to restrict customer instances from certain activities\. Or you might want to ensure that the instances used by employee accounts throughout your organization are granted access to specific internal resources\. For more information, see [Sharing SSM documents](ssm-sharing.md)\.

**SSM document quotas**  
For information about SSM document quotas, see [Systems Manager service quotas](https://docs.aws.amazon.com/general/latest/gr/ssm.html#limits_ssm) in the *Amazon Web Services General Reference*\.

**Topics**
+ [SSM document schemas and features](document-schemas-features.md)
+ [SSM document syntax](sysman-doc-syntax.md)
+ [Systems Manager Command document plugin reference](ssm-plugins.md)
+ [Creating Systems Manager documents](create-ssm-doc.md)
+ [Sharing SSM documents](ssm-sharing.md)
+ [Running Systems Manager command documents from remote locations](run-remote-documents.md)