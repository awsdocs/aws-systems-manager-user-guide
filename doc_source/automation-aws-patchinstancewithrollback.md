# AWS\-PatchInstanceWithRollback<a name="automation-aws-patchinstancewithrollback"></a>

**Description**

Brings Amazon EC2 instance into compliance with standing baseline; rolls back root volume on failure\.

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ InstanceId

  Type: String

  Description: \(Required\) EC2 InstanceId to which we apply the patch\-baseline\.
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf\. If not specified a transient role will be created to execute the Lambda function\.
+ ReportS3Bucket

  Type: String

  Description: \(Optional\) Amazon S3 Bucket destination for the Compliance Report generated during process\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-PatchInstanceWithRollback --parameters InstanceId=i-1234567890abcdef0
```

```
{
    "AutomationExecutionId": "ab08779f-002d-42dd-9222-0123456789ab"
}
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --query 'AutomationExecution.Output'
```

**Document Steps**


****  

| Step Number | Step Name | Automation Action | 
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