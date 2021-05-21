# Working with Change Manager<a name="working-with-change-manager"></a>

With Change Manager, a capability of AWS Systems Manager, users across your organization or in a single Amazon Web Services account can perform change\-related tasks for which they have been granted the necessary permissions\. Change Manager tasks include the following:
+ Create, review, and approve or reject change templates\. 

  A change template is a collection of configuration settings in Change Manager that define such things as required approvals, available runbooks, and notification options for change requests\.
+ Create, review, and approve or reject change requests\.

  A change request is a request in Change Manager to run an Automation runbook that updates one or more resources in your AWS or on\-premises environments\. A change request is created using a change template\.
+ Specify which users in your organization or account can be made reviewers for change templates and change requests\.
+ Edit configuration settings, such as how user identities are managed in Change Manager and which of the available *best practice* options are enforced in your Change Manager operations\. For information about configuring these settings, see [Configuring Change Manager options and best practices](change-manager-account-setup.md)\.

**Topics**
+ [Working with change templates](change-templates.md)
+ [Working with change requests](change-requests.md)
+ [Reviewing change request details, tasks, and timelines \(console\)](reviewing-changes.md)
+ [Viewing aggregated counts of change requests \(command line\)](change-requests-review-aggregate-command-line.md)