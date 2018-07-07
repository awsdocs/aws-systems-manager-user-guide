# Working with Automation Documents<a name="automation-documents"></a>

A Systems Manager Automation document defines the actions that Systems Manager performs on your managed instances and AWS resources\. Documents use JavaScript Object Notation \(JSON\) or YAML, and they include steps and parameters that you specify\. Steps run in sequential order\.

Automation documents are Systems Manager documents of type `Automation`, as opposed to `Command` and `Policy` documents\. Automation documents currently support schema version 0\.3\. Command and Policy documents use schema version 1\.2 or 2\.0\.

**Note**  
To view information about the actions or plugins that you can specify in a Systems Manager Automation document, see [Systems Manager Automation Document Reference](automation-actions.md)\. To view information about the plugins for all other SSM documents, see [SSM Document Plugin Reference](ssm-plugins.md)\.

**Topics**
+ [Working with Predefined Automation Documents](automation-awsdocs.md)
+ [Walkthrough: Create an Automation Document](automation-createdoc.md)