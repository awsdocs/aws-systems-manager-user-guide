# AWS Systems Manager Documents<a name="sysman-ssm-docs"></a>

An AWS Systems Manager document \(SSM document\) defines the actions that Systems Manager performs on your managed instances\. Systems Manager includes more than 100 pre\-configured documents that you can use by specifying parameters at runtime\. You can find pre\-configured documents in the Systems Manager Documents console by choosing the **Owned by Amazon** tab, or by specifying Amazon for the `Owner` filter when calling the `ListDocuments` API operation\. Documents use JavaScript Object Notation \(JSON\) or YAML, and they include steps and parameters that you specify\. To get started with SSM documents, open the [Systems Manager console](https://console.aws.amazon.com/systems-manager/documents)\. In the navigation pane, choose **Documents**\.

## How can the Documents capability benefit my organization?<a name="ssm-docs-benefits"></a>

Documents, a capability of AWS Systems Manager, offers these benefits:
+ **Document categories**

  To help you find the documents you need, choose a category depending on the type of document you're searching for\. To broaden your search, you can choose multiple categories of the same document type\. Choosing categories of different document types is not supported\. Categories are only supported for documents owned by Amazon\.
+  **Document versions** 

  You can create and save different versions of documents\. You can then specify a default version for each document\. The default version of a document can be updated to a newer version or reverted to an older version of the document\. When you change the content of a document, Systems Manager automatically increments the version of the document\. You can retrieve or use any version of a document by specifying the document version in the console, AWS Command Line Interface \(AWS CLI\) commands, or API calls\.
+  **Customize documents for your needs** 

  If you want to customize the steps and actions in a document, you can create your own\. The system stores the document with your AWS account in the AWS Region you create it in\. For more information about how to create an SSM document, see [Creating SSM documents](create-ssm-doc.md)\.
+  **Tag documents** 

  You can tag your documents to help you quickly identify one or more documents based on the tags you've assigned to them\. For example, you can tag documents for specific environments, departments, users, groups, or periods\. You can also restrict access to documents by creating an AWS Identity and Access Management \(IAM\) policy that specifies the tags that a user or group can access\. For more information, see [Tagging Systems Manager documents](tagging-documents.md)\.
+  **Share documents** 

  You can make your documents public or share them with specific AWS accounts in the same AWS Region\. Sharing documents between accounts can be useful if, for example, you want all of the Amazon Elastic Compute Cloud \(Amazon EC2\) instances that you supply to customers or employees to have the same configuration\. In addition to keeping applications or patches on the instances up to date, you might want to restrict customer instances from certain activities\. Or you might want to ensure that the instances used by employee accounts throughout your organization are granted access to specific internal resources\. For more information, see [Sharing SSM documents](ssm-sharing.md)\.

## Who should use Documents?<a name="documents-who"></a>
+ Any AWS customer who wants to use Systems Manager capabilities to improve their operational efficiency at scale, reduce errors associated with manual intervention, and reduce time to resolution of common issues\.
+ Infrastructure experts who want to automate deployment and configuration tasks\.
+ Administrators who want to reliably resolve common issues, improve troubleshooting efficiency, and reduce repetitive operations\.
+ Users who want to automate a task they normally perform manually\.

## What are the types of SSM documents?<a name="what-are-document-types"></a>

The following table describes the different types of SSM documents and their uses\.


****  

| Type | Use with | Details | 
| --- | --- | --- | 
|  ApplicationConfiguration ApplicationConfigurationSchema  |   [AWS AppConfig](https://docs.aws.amazon.com/appconfig/latest/userguide/what-is-appconfig.html)   |  AWS AppConfig, a capability of AWS Systems Manager, enables you to create, manage, and quickly deploy application configurations\. You can store configuration data in an SSM document by creating a document that uses the `ApplicationConfiguration` document type\. For more information, see [Freeform configurations](https://docs.aws.amazon.com/appconfig/latest/userguide/appconfig-creating-configuration-and-profile.html#free-form-configurations) in the *AWS AppConfig User Guide*\.If you create a configuration in an SSM document, then you must specify a corresponding JSON Schema\. The schema uses the `ApplicationConfigurationSchema` document type and, like a set of rules, defines the allowable properties for each application configuration setting\. For more information, see [About validators](https://docs.aws.amazon.com/appconfig/latest/userguide/appconfig-creating-configuration-and-profile-validators.html) in the *AWS AppConfig User Guide*\.  | 
|  Automation runbook  |   [Automation](systems-manager-automation.md)   [State Manager](systems-manager-state.md)   [Maintenance Windows](systems-manager-maintenance.md)   |  Use Automation runbooks when performing common maintenance and deployment tasks such as creating or updating an Amazon Machine Image \(AMI\)\. State Manager uses Automation runbooks to apply a configuration\. These actions can be run on one or more targets at any point during the lifecycle of an instance\. Maintenance Windows uses Automation runbooks to perform common maintenance and deployment tasks based on the specified schedule\. All Automation runbooks that are supported for Linux\-based operating systems are also supported on EC2 instances for macOS\.  | 
|  Change Calendar document  |   [Change Calendar](systems-manager-change-calendar.md)   |  Change Calendar, a capability of AWS Systems Manager, uses the `ChangeCalendar` document type\. A Change Calendar document stores a calendar entry and associated events that can allow or prevent Automation actions from changing your environment\. In Change Calendar, a document stores [iCalendar 2\.0](https://icalendar.org/) data in plaintext format\. Change Calendar isn't supported on EC2 instances for macOS\.  | 
|  AWS CloudFormation template  |   [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)   |  AWS CloudFormation templates describe the resources that you want to provision in your CloudFormation stacks\. By storing CloudFormation templates as Systems Manager documents, you can benefit from Systems Manager document features\. These include creating and comparing multiple versions of your template, and sharing your template with other accounts in the same AWS Region\. You can create and edit CloudFormation templates and stacks by using Application Manager, a capability of Systems Manager\. For more information, see [Working with AWS CloudFormation templates and stacks in Application Manager](application-manager-working-stacks.md)\.  | 
|  Command document  |   [Run Command](run-command.md)   [State Manager](systems-manager-state.md)   [Maintenance Windows](systems-manager-maintenance.md)   |  Run Command, a capability of AWS Systems Manager, uses Command documents to run commands\. State Manager, a capability of AWS Systems Manager, uses command documents to apply a configuration\. These actions can be run on one or more targets at any point during the lifecycle of an instance\. Maintenance Windows, a capability of AWS Systems Manager, uses Command documents to apply a configuration based on the specified schedule\. Most Command documents are supported on all Linux and Windows Server operating systems supported by Systems Manager\. The following Command documents are supported on EC2 instances for macOS: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-ssm-docs.html)  | 
|  AWS Config conformance pack template  |   [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html)   |  AWS Config conformance pack templates are YAML formatted documents used to create conformance packs that contains the list of AWS Config managed or custom rules and remediation actions\. For more information, see [Conformance Packs](https://docs.aws.amazon.com/config/latest/developerguide/conformance-packs.html)\.  | 
|  Package document  |   [Distributor](distributor.md)   |  In Distributor, a capability of AWS Systems Manager, a package is represented by an SSM document\. A package document includes attached ZIP archive files that contain software or assets to install on managed instances\. Creating a package in Distributor creates the package document\. Distributor isn't supported on Oracle Linux and macOS managed instances\.  | 
|  Policy document  |   [State Manager](systems-manager-state.md)   |  Inventory, a capability of AWS Systems Manager, uses the `AWS-GatherSoftwareInventory` Policy document with a State Manager association to collect inventory data from managed instances\. When creating your own SSM documents, Automation runbooks and Command documents are the preferred method for enforcing a policy on a managed instance\. Systems Manager Inventory and the `AWS-GatherSoftwareInventory` Policy document are supported on all operating systems supported by Systems Manager\.  | 
|  Post\-incident analysis template  |   [Incident Manager post\-incident analysis](https://docs.aws.amazon.com/incident-manager/latest/userguide/analysis.html)   |  Incident Manager uses the post\-incident analysis template to create an analysis based on AWS operations management best practices\. Use the template to create an analysis that your team can use to identify improvements to your incident response\.   | 
|  Session document  |   [Session Manager](session-manager.md)   |  Session Manager, a capability of AWS Systems Manager, uses Session documents to determine which type of session to start, such as a port forwarding session, a session to run an interactive command, or a session to create an SSH tunnel\. Session documents are supported on all Linux and Windows Server operating systems supported by Systems Manager\. The following Command documents are supported on EC2 instances for macOS: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-ssm-docs.html)  | 

**SSM document quotas**  
For information about SSM document quotas, see [Systems Manager service quotas](https://docs.aws.amazon.com/general/latest/gr/ssm.html#limits_ssm) in the *Amazon Web Services General Reference*\.

**Topics**
+ [How can the Documents capability benefit my organization?](#ssm-docs-benefits)
+ [Who should use Documents?](#documents-who)
+ [What are the types of SSM documents?](#what-are-document-types)
+ [SSM document schema features and examples](document-schemas-features.md)
+ [SSM document data elements](sysman-doc-syntax.md)
+ [Systems Manager Command document plugin reference](ssm-plugins.md)
+ [Viewing SSM Command document content](viewing-ssm-document-content.md)
+ [Creating SSM documents](create-ssm-doc.md)
+ [Deleting custom SSM documents](ssm-deleting.md)
+ [Comparing SSM document versions](ssm-comparing.md)
+ [Sharing SSM documents](ssm-sharing.md)
+ [Searching for SSM documents](ssm-documents-searching.md)
+ [Running Systems Manager Command documents from remote locations](run-remote-documents.md)