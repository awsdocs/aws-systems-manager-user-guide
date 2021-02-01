# Amazon managed runbooks that run scripts<a name="runbook-scripts"></a>

AWS Systems Manager Automation runbooks support running scripts as part of the automation\. 

The following are AWS managed runbooks that include support for running scripts\.

**Note**  
You can also create your own custom runbooks that can run scripts\. For information, see [Creating runbooks that run scripts](automation-document-script.md)\.
+ [AWS\-CreateRdsSnapshot](automation-aws-createrdssnapshot.md) – Creates an Amazon Relational Database Service \(Amazon RDS\) snapshot for an Amazon RDS instance\. 
+ [ AWS\-CreateServiceNowIncident](automation-aws-createservicenowincident.md) – Creates an incident in the ServiceNow incident table\.
+ [AWS\-ExportOpsDataToS3](automation-aws-exportopsdatatos3.md) – Retrieves a list of OpsData summaries in AWS Systems Manager Explorer and exports them to an object in a specified S3 bucket\. 
+ [AWS\-RunCfnLint](automation-aws-runcfnlint.md) – Uses an [AWS CloudFormation Linter](https://github.com/aws-cloudformation/cfn-python-lint) \(cfn\-python\-lint\) to validate YAML and JSON templates against the AWS CloudFormation resource specification\. This runbook is using cfn\-lint v0\.24\.4\. 
+ [AWS\-RunPacker](automation-aws-runpacker.md) – Uses the HashiCorp [Packertool](https://www.packer.io/) tool to validate, fix, or build packer templates that are used to create machine images\. This runbook is using Packer v1\.4\.4\. 