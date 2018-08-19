# Restrict Access to Root\-Level Commands Through SSM Agent<a name="ssm-agent-restrict-root-level-commands"></a>

SSM Agent runs on Amazon EC2 instances using root permissions \(Linux\) or SYSTEM permissions \(Windows\)\. Because there are the highest level of system access privileges, any trusted entity that has been granted permission to send commands to SSM Agent has root or SYSTEM permissions\. \(In AWS, a trusted entity that can perform actions and access resources in AWS is called a principal\. A principal can be an AWS account root user, an IAM user, or a role\.\)

This level of access is required for a principal to send authorized Systems Manager commands to SSM Agent, but also makes it possible for a principal to run malicious code by exploiting any potential vulnerabilities in SSM Agent\. 

In particular, permissions to run the command [SendCommand](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendCommand.html) should be carefully restricted\. A good first step is to grant permission for the command only to select principals in your organization\. However, we recommend tightening your security posture even further by restricting which instances a principal can run his command on\. This can be done in the IAM user policy assigned to the principal\. In the IAM policy, you can include a condition that limits the user to running commands only on instances that are tagged with specific Amazon EC2 tags, or combinations of EC2 tags\.

For example, say you have two fleets of instances, one for testing, one for production\. In the IAM policy applied to junior engineers, you specify that they can run commands only on instances tagged with `ssm:resourceTag/testServer`\. But for a smaller group of lead engineers, who should have access to all instances, you grant access to instances tagged with both `ssm:resourceTag/testServer` and `ssm:resourceTag/productionServer`\.

Using this approach, if junior engineers attempt to run a command on a production instance, they will be denied access because their assigned IAM policy does not provide explicit access to instances tagged with `ssm:resourceTag/productionServer`\.

For more information see [Restricting Run Command Access Based on Instance Tags](sysman-rc-setting-up-cmdsec.md)\.