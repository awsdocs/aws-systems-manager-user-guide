# AWS\-RunPacker<a name="automation-aws-runpacker"></a>

**Description**

This document uses the HashiCorp [Packer](https://www.packer.io/) tool to validate, fix, or build packer templates that are used to create machine images\. This document is using Packer v1\.4\.4\.

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ TemplateS3BucketName

  Type: String

  Description: The name of the Amazon S3 bucket containing the packer template\.
+ TemplateFileName

  Type: String

  Description: The name, or key, of the template file in the S3 bucket\.
+ Mode

  Type: String

  Description: The mode, or command, in which to use Packer when validating against the template\. Options include `Build`, `Validate`, and `Fix`\.
+ Force

  Type: Boolean

  Description: A Packer option to force a builder to run when artifacts from a previous build otherwise prevent a build from running\. 
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-RunPacker \
--parameters "TemplateS3BucketName=MyBucket,TemplateFileName=MyTemplate,Mode=Fix,Force=False,AutomationAssumeRole=arn:aws:iam::111122223333:role/AutomationServiceRole"
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'RunPackerProcessTemplate.output'
```

**Document Steps**

RunPackerProcessTemplate – Runs the selected mode against the template using the Packer tool\.

**Outputs**

RunPackerProcessTemplate\.output – The stdout from the Packer tool\.

RunPackerProcessTemplate\.fixed\_template\_key – The name of the template stored in an Amazon S3 bucket to use only when running in "Fix" mode\.

RunPackerProcessTemplate\.s3\_bucket – The name of the S3 bucket that contains the fixed template to use only when running in "Fix" mode\.