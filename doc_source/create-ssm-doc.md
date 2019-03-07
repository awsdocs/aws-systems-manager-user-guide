# Creating Systems Manager Documents<a name="create-ssm-doc"></a>

If the Systems Manager public documents limit the actions you want to perform on your managed instances, you can create your own documents\. When creating a new document, we recommend that you use schema version 2\.2 or later\. 

**Before You Begin**  
Before you create an SSM document, we recommend that you read about the different schemas, features, and syntax available for SSM documents\. For more information, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

**Note**  
If you plan to create an SSM document for State Manager, be aware of the following details:  
You can assign multiple documents to a target by creating different State Manager associations that use different documents\. 
If you create a document with conflicting plugins \(e\.g\., domain join and remove from domain\), the last plugin executed will be the final state\. State Manager does not validate the logical sequence or rationality of the commands or plugins in your document\.
When processing documents, instance associations are applied first, and next tagged group associations are applied\. If an instance is part of multiple tagged groups, then the documents that are part of the tagged group will not be executed in any particular order\. If an instance is directly targeted through multiple documents by its instance ID, there is no particular order of execution\. 
If you change the default version of an SSM Policy document for State Manager, any association that uses the document will start using the new default version the next time Systems Manager applies the association to the instance\.
If you create an association using an SSM document that was shared with you, and then the owner stops sharing the document with you, your associations no longer have access to that document\. However, if the owner shares the same SSM document with you again later, your associations automatically remap to it\.
If you create an SSM document for State Manager, you must associate the document with your managed instances after you add it to the system\. For more information, see [Create an Association \(Console\)](sysman-state-assoc.md)\.

**Topics**
+ [Copy a Document](copy-document.md)
+ [Add a Systems Manager Document \(Console\)](create-ssm-console.md)
+ [Create an SSM Document \(AWS CLI\)](create-ssm-document-cli.md)
+ [Create an SSM Document \(Tools for Windows PowerShell\)](create-ssm-document-ps.md)