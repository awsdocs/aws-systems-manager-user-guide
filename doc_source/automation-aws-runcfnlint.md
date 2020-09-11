# AWS\-RunCfnLint<a name="automation-aws-runcfnlint"></a>

**Description**

This document uses an [AWS CloudFormation Linter](https://github.com/aws-cloudformation/cfn-python-lint) \(`cfn-python-lint`\) to validate YAML and JSON templates against the AWS CloudFormation resource specification\. The `AWS-RunCfnLint` document performs additional checks, such as ensuring that valid values have been entered for resource properties\. If validation is not successful, the `RunCfnLintAgainstTemplate` step fails and the linter tool's output is provided in an error message\. This Automation document is using cfn\-lint v0\.24\.4\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-RunCfnLint)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ ConfigureRuleFlag

  Type: String

  Description: \(Optional\) Configuration options for a rule to pass to the \-\-configure\-rule parameter\. 

  Example: E2001:strict=false,E3012:strict=false\.
+ FormatFlag

  Type: String

  Description: \(Optional\) Value to pass to the \-\-format parameter to specify the output format\.

  Valid values: Default \| quiet \| parseable \| json

  Default: Default
+ IgnoreChecksFlag

  Type: String

  Description: \(Optional\) IDs of rules to pass to the \-\-ignore\-checks parameter\. These rules are not checked\.

  Example: E1001,E1003,W7001
+ IncludeChecksFlag

  Type: String

  Description: \(Optional\) IDs of rules to pass to the \-\-include\-checks parameter\. These rules are checked\.

  Example: E1001,E1003,W7001
+ InfoFlag

  Type: String

  Description: \(Optional\) Option for the \-\-info parameter\. Include the option to enable additional logging information about the template processing\.

  Default: False
+ TemplateFileName

  Type: String

  Description: The name, or key, of the template file in the S3 bucket\.
+ TemplateS3BucketName

  Type: String

  Description: The name of the S3 bucket containing the packer template\.
+ RegionsFlag

  Type: String

  Description: \(Optional\) Values to pass to the for \-\-regions parameter to test the template against specified AWS Regions\.

  Example: us\-east\-1,us\-west\-1

**Document Steps**

RunCfnLintAgainstTemplate – Runs the `cfn-python-lint` tool against the specified AWS CloudFormation template\.

**Outputs**

RunCfnLintAgainstTemplate\.output – The stdout from the `cfn-python-lint` tool\.