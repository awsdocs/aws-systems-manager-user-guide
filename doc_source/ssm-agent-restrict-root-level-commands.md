# Restricting access to root\-level commands through SSM Agent<a name="ssm-agent-restrict-root-level-commands"></a>

AWS Systems Manager Agent \(SSM Agent\) runs on Amazon Elastic Compute Cloud \(Amazon EC2\) instances using root permissions \(Linux\) or SYSTEM permissions \(Windows Server\)\. Because these are the highest level of system access permissions, any trusted entity that has been granted permission to send commands to SSM Agent has root or SYSTEM permissions\. \(In AWS, a trusted entity that can perform actions and access resources in AWS is called a *principal*\. A principal can be an AWS account root user, user, or a role\.\)

This level of access is required for a principal to send authorized Systems Manager commands to SSM Agent, but also makes it possible for a principal to run malicious code by exploiting any potential vulnerabilities in SSM Agent\. 

In particular, permissions to run the commands [SendCommand](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendCommand.html) and [StartSession](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_StartSession.html) should be carefully restricted\. A good first step is to grant permissions for each command only to select principals in your organization\. However, we recommend tightening your security posture even further by restricting which managed nodes a principal can run these commands on\. This can be done in the IAM policy assigned to the principal\. In the IAM policy, you can include a condition that limits the user to running commands only on managed nodes that are tagged with specific tags or a combination of tags\.

For example, say you have two fleets of EC2 instances, one for testing, one for production\. In the IAM policy applied to junior engineers, you specify that they can run commands only on instances tagged with `ssm:resourceTag/testServer`\. But, for a smaller group of lead engineers, who should have access to all instances, you grant access to instances tagged with both `ssm:resourceTag/testServer` and `ssm:resourceTag/productionServer`\.

Using this approach, if junior engineers attempt to run a command on a production instance, they will be denied access because their assigned IAM policy doesn't provide explicit access to instances tagged with `ssm:resourceTag/productionServer`\.

For more information and examples, see the following topics:
+ [Restricting Run Command access based on tags](run-command-setting-up.md#tag-based-access)
+ [Restrict session access based on instance tags](getting-started-restrict-access-examples.md#restrict-access-example-instance-tags)