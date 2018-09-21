# AWS\-DeleteCloudFormationStack<a name="automation-aws-deletecloudformationstack"></a>

**Description**

Delete an AWS CloudFormation stack\.

**Document Type**

Automation

**Owner**

Amazon

**Platform\(s\)**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ StackNameOrId

  Type: String

  Description: \(Required\) Name or Unique ID of the CloudFormation stack to be deleted

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-DeleteCloudFormationStack --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

aws:deleteStack

**Outputs**

None