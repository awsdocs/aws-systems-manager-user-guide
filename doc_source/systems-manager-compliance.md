# AWS Systems Manager Configuration Compliance<a name="systems-manager-compliance"></a>

You can use AWS Systems Manager Configuration Compliance to scan your fleet of managed instances for patch compliance and configuration inconsistencies\. You can collect and aggregate data from multiple AWS accounts and Regions, and then drill down into specific resources that aren’t compliant\. By default, Configuration Compliance displays compliance data about Systems Manager Patch Manager patching and Systems Manager State Manager associations\. You can also customize the service and create your own compliance types based on your IT or business requirements\. You can quickly remediate issues using Systems Manager Run Command, State Manager, or Amazon CloudWatch Events\. You can also port data to Amazon Athena and Amazon QuickSight to generate fleet\-wide reports\. 

Configuration Compliance is offered at no additional charge\. You only pay for the AWS resources that you use\.

**Note**  
Systems Manager now integrates with [Chef InSpec](https://www.chef.io/inspec/)\. InSpec is an open\-source, runtime framework that enables you to create human\-readable profiles on GitHub or Amazon S3\. Then you can use Systems Manager to run compliance scans and view compliant and noncompliant instances\. For more information, see [Using Chef InSpec Profiles with Systems Manager Compliance](integration-chef-inspec.md)\.

**Topics**
+ [Getting Started with Configuration Compliance](sysman-compliance-prereqs.md)
+ [About Configuration Compliance](sysman-compliance-about.md)
+ [Remediating Compliance Issues](sysman-compliance-fixing.md)
+ [Systems Manager Configuration Compliance Manager Walkthrough \(AWS CLI\)](sysman-compliance-walk.md)