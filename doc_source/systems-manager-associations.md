# Working with Associations in Systems Manager<a name="systems-manager-associations"></a>

A State Manager association is a configuration that is assigned to your managed instances\. The configuration defines the state that you want to maintain on your instances\. For example, an association can specify that anti\-virus software must be installed and running on your instances, or that certain ports must be closed\. The association specifies a schedule for when the configuration is reapplied\. The association also specifies actions to take when applying the configuration\. For example, an association for anti\-virus software might run once a day\. If the software is not installed, then State Manager installs it\. If the software is installed, but the service is not running, then the association might instruct State Manager to start the service\.

Use the following topics to help you create and manage State Manager associations\.

**Topics**
+ [Create an Association](sysman-state-assoc.md)
+ [Using Targets and Rate Controls with State Manager Associations](systems-manager-state-manager-targets-and-rate-controls.md)
+ [Edit and Create a New Version of an Association \(Console\)](sysman-state-assoc-version.md)
+ [Viewing Association Histories](sysman-state-assoc-history.md)