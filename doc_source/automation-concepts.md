# AWS Systems Manager Automation Concepts<a name="automation-concepts"></a>

AWS Systems Manager Automation uses the following concepts\.


****  

| Concept | Details | 
| --- | --- | 
|  Automation document  |  A Systems Manager Automation document defines the Automation workflow \(the actions that Systems Manager performs on your managed instances and AWS resources\)\. Automation includes several pre\-defined Automation documents that you can use to perform common tasks like restarting one or more Amazon EC2 instances or creating an Amazon Machine Image \(AMI\)\. Documents use JavaScript Object Notation \(JSON\) or YAML, and they include steps and parameters that you specify\. Steps run in sequential order\. For more information, see [Working with Automation Documents](automation-documents.md)\.  | 
|  Automation action  |  The Automation workflow defined in an Automation document includes one or more steps\. Each step is associated with a particular action or plugin\. The action determines the inputs, behavior, and outputs of the step\. Steps are defined in the `mainSteps` section of your Automation document\. For more information, see the [Systems Manager Automation Document Reference](automation-actions.md)\.  | 
|  Automation queue  |  Each AWS account can run 25 Automations simultaneously\. If you attempt to run more than this, Systems Manager adds the additional executions to a queue and displays a status of Pending\. When an Automation completes \(or reaches a terminal state\), the first execution in the queue starts\. Each AWS account can queue 75 Automation executions\.  | 