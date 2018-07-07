# SSM Document Schemas and Features<a name="document-schemas-features"></a>

Systems Manager documents currently use the following schema versions\.
+ Documents of type `Command` can use schema version 1\.2, 2\.0, and 2\.2\. If you are currently using schema 1\.2 documents, we recommend that you create documents that use schema version 2\.2\.
+ Documents of type `Policy` must use schema version 2\.0 or later\.
+ Documents of type `Automation` must use schema version 0\.3\.
+ You can create documents in JSON or YAML\.

By using the latest schema version for `Command` and `Policy` documents, you can take advantage of the following features\.


**Schema Version 2\.2 Document Features**  

| Feature | Details | 
| --- | --- | 
|  Document editing  |  Documents can now be updated\. With version 1\.2, any update to a document required that you save it with a different name\.  | 
|  Automatic versioning  |  Any update to a document creates a new version\. This is not a schema version, but a version of the document\.  | 
|  Default version  |  If you have multiple versions of a document, you can specify which version is the default document\.  | 
|  Sequencing  |  Plugins or *steps* in a document execute in the order that you specified\.  | 
|  Cross\-platform support  |  Cross\-platform support enables you to specify different operating systems for different plugins within the same SSM document\. Cross\-platform support uses the `precondition` parameter within a step\.   | 

**Note**  
You must keep SSM Agent on your instances updated with the latest version to use new Systems Manager features and SSM document features\. For more information, see [Example: Update the SSM Agent](rc-console.md#rc-console-agentexample)\.

The following table lists the differences between major schema versions\.


****  

| Version 1\.2 | Version 2\.2 \(latest version\) | Details | 
| --- | --- | --- | 
|  runtimeConfig  |  mainSteps  |  In version 2\.2, the `mainSteps` section replaces `runtimeConfig`\. The `mainSteps` section enables Systems Manager to execute steps in sequence\.  | 
|  properties  |  inputs  |  In version 2\.2, the `inputs` section replaces the `properties` section\. The `inputs` section accepts parameters for steps\.  | 
|  commands  |  runCommand  |  In version 2\.2, the `inputs` section takes the `runCommand` parameter instead of the `commands` parameter\.  | 
|  id  |  action  |  In version 2\.2, `Action` replaces `ID`\. This is just a name change\.  | 
|  not applicable  |  name  |  In version 2\.2, `name` is any user\-defined name for a step\.  | 

**Using the Precondition Parameter**  
With schema version 2\.2 or higher, you can use the `precondition` parameter to specify the target operating system for each plugin\. The `precondition` parameter supports `platformType` and a value of either `Windows` or `Linux`\.

For documents that use schema version 2\.2 or higher, if `precondition` is not specified, each plugin is either executed or skipped based on the plugin’s compatibility with the operating system\. For documents that use schema 2\.0 or earlier, incompatible plugins throw an error\.

For example, in a schema version 2\.2 document, if `precondition` is not specified and the `aws:runShellScript` plugin is listed, then the step runs on Linux instances, but the system skips it on Windows instances because the `aws:runShellScript` is not compatible with Windows instances\. However, for a schema version 2\.0 document, if you specify the `aws:runShellScript` plugin, and then run the document on a Windows instances, the execution fails\. You can see an example of the the precondition parameter in an SSM document later in this section\.