# AWS\-DeleteImage<a name="automation-aws-deleteimage"></a>

**Description**

Delete an Amazon Machine Image \(AMI\) and all associated snapshots\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-DeleteImage)

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
+ ImageId

  Type: String

  Description: \(Required\) The ID of the AMI\.

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully execute the Automation document:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:DeleteSnapshot",
            "Resource": "arn:aws:ec2:{region}::snapshot/*"
        },
        {
            "Effect": "Allow",
            "Action": "ec2:DescribeImages",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "ec2:DeregisterImage",
            "Resource": "*"
        }
    ]
}
```