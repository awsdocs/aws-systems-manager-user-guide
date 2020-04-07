# AWS\-DisablePublicAccessForSecurityGroup<a name="automation-aws-disablepublicaccessforsecuritygroup"></a>

 **Description** 

This document disables default SSH and RDP ports that are opened to all IP addresses\.

**Important**  
This document fails with an "InvalidPermission\.NotFound" error for security groups that meet both of the following criteria: 1\) The security group is located in a non\-default VPC; and 2\) The inbound rules for the security group don't specify open ports using all four of the following patterns:   
`0.0.0.0/0`
`::/0`
`SSH or RDP port + 0.0.0.0/0`
`SSH or RDP port + ::/0`
If the security group is located in a non\-default VPC and, for example, specifies open ports using only the `SSH or RDP port + 0.0.0.0/0` format, then the document fails to run\. 

 **Document Type** 

Automation

 **Owner** 

Amazon

 **Platform\(s\)** 

Windows, Linux

 **Parameters** 
+ GroupId

  Type: String

  Description: \(Required\) The ID of the security group for which the ports should be disabled\.
+ IpAddressToBlock

  Type: String

  Description: \(Optional\) Additional IPv4 addresses from which access should be blocked, in the format `1.2.3.4/32`\.
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.

 **Examples** 

Start the automation

```
aws ssm start-automation-execution --document-name AWS-DisablePublicAccessForSecurityGroup --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

 **Outputs** 

None