# AWS\-RunPacker<a name="automation-aws-runpacker"></a>

**Description**

This document uses the HashiCorp [Packer](https://www.packer.io/) tool to validate, fix, or build packer templates that are used to create machine images\. This document uses Packer v1\.4\.4\.

**Note**  
If you specify a `vpc_id` value, you must also specify the `subnet_id` value of a public subnet\. Unless you modify your subnet's IPv4 public addressing attribute, you must also set `associate_public_ip_address` to true\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-RunPacker)

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
+ Force

  Type: Boolean

  Description: A Packer option to force a builder to run when artifacts from a previous build otherwise prevent a build from running\. 
+ Mode

  Type: String

  Description: The mode, or command, in which to use Packer when validating against the template\. Options include `Build`, `Validate`, and `Fix`\.
+ TemplateFileName

  Type: String

  Description: The name, or key, of the template file in the S3 bucket\.
+ TemplateS3BucketName

  Type: String

  Description: The name of the S3 bucket containing the packer template\.

**Document Steps**

RunPackerProcessTemplate – Runs the selected mode against the template using the Packer tool\.

**Outputs**

RunPackerProcessTemplate\.output – The stdout from the Packer tool\.

RunPackerProcessTemplate\.fixed\_template\_key – The name of the template stored in an S3 bucket to use only when running in "Fix" mode\.

RunPackerProcessTemplate\.s3\_bucket – The name of the S3 bucket that contains the fixed template to use only when running in "Fix" mode\.