# Setting up Parameter Store<a name="paramstore-setting-up"></a>

Before setting up parameters in Systems Manager Parameter Store, you must first configure AWS Identity and Access Management \(IAM\) policies that provide users in your account with permission to perform the actions you specify\. This section includes information about how to manually configure these policies using the IAM console, and how to assign them to users and user groups\. You can also create and assign policies to control which parameter actions can be run on an instance\. This section also include information about how to create Amazon EventBridge rules that let you receive notifications about changes to Systems Manager parameters\. You can also use EventBridge rules to trigger other actions in AWS based on changes in Parameter Store\.

**Topics**
+ [Restricting access to Systems Manager parameters using IAM policies](paramstore-access.md)
+ [Managing parameter tiers](paramstore-advanced-parameters.md)
+ [Increasing Parameter Store throughput](paramstore-throughput.md)
+ [Setting up notifications or trigger actions based on Parameter Store events](paramstore-cwe.md)