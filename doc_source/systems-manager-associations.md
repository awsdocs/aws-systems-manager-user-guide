# Working with associations in Systems Manager<a name="systems-manager-associations"></a>

A State Manager association is a configuration that is assigned to your managed instances\. The configuration defines the state that you want to maintain on your instances\. For example, an association can specify that antivirus software must be installed and running on your instances, or that certain ports must be closed\. The association specifies a schedule for when the configuration is applied once or reapplied at specified times\. The association also specifies actions to take when applying the configuration\. For example, an association for antivirus software might run once a day\. If the software is not installed, then State Manager installs it\. If the software is installed, but the service is not running, then the association might instruct State Manager to start the service\.

Use the following topics to help you create and manage State Manager associations\.

**Topics**
+ [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)
+ [Create an association](sysman-state-assoc.md)
+ [Edit and create a new version of an association](sysman-state-assoc-edit.md)
+ [Viewing association histories](sysman-state-assoc-history.md)