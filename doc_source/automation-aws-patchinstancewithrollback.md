# AWS\-PatchInstanceWithRollback<a name="automation-aws-patchinstancewithrollback"></a>

**Description**

Brings EC2 instance into compliance with standing baseline; rolls back root volume on failure\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-PatchInstanceWithRollback)

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
+ InstanceId

  Type: String

  Description: \(Required\) EC2 InstanceId to which we apply the patch\-baseline\.
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf\. If not specified a transient role will be created to run the Lambda function\.
+ ReportS3Bucket

  Type: String

  Description: \(Optional\) Amazon S3 Bucket destination for the Compliance Report generated during process\.

**Document Steps**


****  

| Step number | Step name | Automation action | 
| --- | --- | --- | 
|  1  |  createDocumentStack  |  aws:createStack  | 
|  2  |  IdentifyRootVolume  |  aws:invokeLambdaFunction  | 
|  3  |  PrePatchSnapshot  |  aws:executeAutomation  | 
|  4  |  installMissingUpdates  |  aws:runCommand  | 
|  5  |  SleepThruInstallation  |  aws:invokeLambdaFunction  | 
|  6  |  CheckCompliance  |  aws:invokeLambdaFunction  | 
|  7  |  SaveComplianceReportToS3  |  aws:invokeLambdaFunction  | 
|  8  |  ReportSuccessOrFailure  |  aws:invokeLambdaFunction  | 
|  9  |  DeleteSnapshot  |  aws:invokeLambdaFunction  | 
|  10  |  createDocumentStack  |  aws:deleteStack  | 

**Outputs**

IdentifyRootVolume\.Payload

PrePatchSnapshot\.Output

SaveComplianceReportToS3\.Payload

RestoreFromSnapshot\.Payload

CheckCompliance\.Payload