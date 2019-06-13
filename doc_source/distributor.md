# AWS Systems Manager Distributor<a name="distributor"></a>

AWS Systems Manager Distributor lets you package your own software—or find AWS\-provided agent software packages, such as **AmazonCloudWatchAgent**—to install on AWS Systems Manager managed instances\. Distributor publishes resources, such as software packages, to AWS Systems Manager managed instances\. Publishing a package advertises specific versions of the package's document—a Systems Manager [document](sysman-ssm-docs.md) that you create when you add the package in Distributor—to managed instances that you identify by managed instance IDs, AWS account IDs, tags, or an AWS Region\.

After you create a package in Distributor, which creates an AWS Systems Manager document, you can install the package in one of the following ways\.
+ One time by using [AWS Systems Manager Run Command](execute-remote-commands.md)\.
+ On a schedule by using [AWS Systems Manager State Manager](systems-manager-state.md)\.

**Topics**
+ [Learn More About Distributor](what-is-distributor.md)
+ [Getting Started with Distributor](distributor-getting-started.md)
+ [Working with Distributor](distributor-working-with.md)
+ [Auditing and Logging Distributor Activity](distributor-logging-auditing.md)
+ [Troubleshooting AWS Systems Manager Distributor](distributor-troubleshooting.md)