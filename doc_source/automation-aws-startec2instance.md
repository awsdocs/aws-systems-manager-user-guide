# AWS\-StartEC2Instance<a name="automation-aws-startec2instance"></a>

**Description**

Start one or more EC2 instances\.

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

  Type: StringList

  Description: \(Required\) EC2 instances to start\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-StartEC2Instance --parameters "InstanceId=INSTANCEID"
```

**Outputs**

The automation execution has no output\.