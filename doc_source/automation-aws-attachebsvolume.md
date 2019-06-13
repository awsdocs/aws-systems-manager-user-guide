# AWS\-AttachEBSVolume<a name="automation-aws-attachebsvolume"></a>

**Description**

Attach an Amazon Elastic Block Store \(Amazon EBS\) volume to an Amazon EC2 instance\.

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
+ Device

  Type: String

  Description: \(Required\) The device name \(for example, /dev/sdh or xvdh \)\.
+ InstanceId

  Type: String

  Description: \(Required\) The ID of the instance where you want to attach the volume\.
+ VolumeId

  Type: String

  Description: \(Required\) The ID of the Amazon EBS volume\. The volume and instance must be in the same Availability Zone\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-AttachEBSVolume --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

aws:createStack

aws:deleteStack

**Outputs**

None