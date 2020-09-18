# AWS Systems Manager Configuration Compliance<a name="systems-manager-compliance"></a>

You can use AWS Systems Manager Configuration Compliance to scan your fleet of managed instances for patch compliance and configuration inconsistencies\. You can collect and aggregate data from multiple AWS accounts and Regions, and then drill down into specific resources that aren’t compliant\. By default, Configuration Compliance displays current compliance data about Systems Manager Patch Manager patching and Systems Manager State Manager associations\. Systems Manager Compliance offers the following additional benefits and features:
+ View compliance history and change tracking for Patch Manager patching data and State Manager associations by using AWS Config\.
+ Customize Systems Manager Compliance to create your own compliance types based on your IT or business requirements\.
+ Remediate issues by using Systems Manager Run Command, State Manager, or Amazon EventBridge\.
+ Port data to Amazon Athena and Amazon QuickSight to generate fleet\-wide reports\.

**Amazon EventBridge support**  
This Systems Manager capability is supported as an *event* type in EventBridge rules\. For information, see [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md) and [Reference: Amazon EventBridge event patterns and types for Systems Manager](reference-eventbridge-events.md)\.

**Chef InSpec integration**  
Systems Manager integrates with [Chef InSpec](https://www.chef.io/inspec/)\. InSpec is an open\-source, runtime framework that enables you to create human\-readable profiles on GitHub or Amazon S3\. Then you can use Systems Manager to run compliance scans and view compliant and noncompliant instances\. For more information, see [Using Chef InSpec profiles with Systems Manager Compliance](integration-chef-inspec.md)\.

**Pricing**  
Configuration Compliance is offered at no additional charge\. You only pay for the AWS resources that you use\.

**Topics**
+ [Getting started with Configuration Compliance](sysman-compliance-prereqs.md)
+ [Creating a Resource Data Sync for Configuration Compliance](sysman-compliance-datasync-create.md)
+ [Working with Configuration Compliance](sysman-compliance-about.md)
+ [Remediating compliance issues using EventBridge](sysman-compliance-fixing.md)
+ [Configuration Compliance walkthrough \(AWS CLI\)](sysman-compliance-walk.md)