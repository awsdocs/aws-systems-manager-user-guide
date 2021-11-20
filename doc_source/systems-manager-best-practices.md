# Use cases and best practices<a name="systems-manager-best-practices"></a>

This topic lists common use cases and best practices for AWS Systems Manager capabilities\. If available, this topic also includes links to relevant blog posts and technical documentation\.

**Note**  
The title of each section here is an active link to the corresponding section in the technical documentation\.

**[Automation](systems-manager-automation.md)**
+ Create self\-service Automation runbooks for infrastructure\.
+ Use Automation, a capability of AWS Systems Manager, to simplify creating Amazon Machine Images \(AMIs\) from the AWS Marketplace or custom AMIs, using public Systems Manager documents \(SSM documents\) or by authoring your own workflows\.
+ [Build and maintain AMIs](automation-walk.md) using the `AWS-UpdateLinuxAmi` and `AWS-UpdateWindowsAmi` Automation runbooks, or using custom Automation runbooks that you create\.

**[Inventory](systems-manager-inventory.md)**
+ Use Inventory, a capability of AWS Systems Manager, with AWS Config to audit your application configurations over time\.

**[Maintenance Windows](systems-manager-maintenance.md)**
+ Define a schedule to perform potentially disruptive actions on your instances such as operating system \(OS\) patching, driver updates, or software installations\.
+ For information about the differences between State Manager and Maintenance Windows, capabilities of AWS Systems Manager, see [Choosing between State Manager and Maintenance Windows](state-manager-vs-maintenance-windows.md)\.

**[Parameter Store](systems-manager-parameter-store.md)**
+ Use Parameter Store, a capability of AWS Systems Manager, to centrally manage global configuration settings\.
+ [How AWS Systems Manager Parameter Store uses AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/services-parameter-store.html)\.
+ [Reference AWS Secrets Manager secrets from Parameter Store parameters](integration-ps-secretsmanager.md)\.

**[Patch Manager](systems-manager-patch.md)**
+ Use Patch Manager, a capability of AWS Systems Manager, to roll out patches at scale and increase fleet compliance visibility across your instances\.
+  [Integrate Patch Manager with AWS Security Hub](patch-manager-security-hub-integration.md) to receive alerts when instances in your fleet go out of compliance and monitor the patching status of your fleets from a security point of view\. There is a charge to use Security Hub\. For more information, see [Pricing](http://aws.amazon.com/security-hub/pricing/)\.

**[Run Command](execute-remote-commands.md)**
+ [Manage Instances at Scale without SSH Access Using EC2 Run Command](http://aws.amazon.com/blogs/aws/manage-instances-at-scale-without-ssh-access-using-ec2-run-command/)\.
+ Audit all API calls made by or on behalf of Run Command, a capability of AWS Systems Manager, using AWS CloudTrail\.
+ When you run a command in Run Command, don't include any sensitive information formatted as plaintext, such as passwords, configuration data, or other secrets\. All Systems Manager API activity in your account is logged in an Amazon S3 bucket, in AWS CloudTrail logs\. This means that any user with access to that S3 bucket can view the plaintext values of those secrets\. For this reason, we strongly recommend creating and using `SecureString` parameters to encrypt the sensitive data you use in your Systems Manager operations\.

  For more information, see [Restricting access to Systems Manager parameters using IAM policies](sysman-paramstore-access.md)\.
**Note**  
By default, the log files delivered by CloudTrail to your bucket are encrypted by Amazon [server\-side encryption with Amazon S3\-managed encryption keys \(SSE\-S3\)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingServerSideEncryption.html)\. To provide a security layer that is directly manageable, you can instead use [server\-side encryption with AWS KMS–managed keys \(SSE\-KMS\)](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html) for your CloudTrail log files\.  
For more information, see [Encrypting CloudTrail log files with AWS KMS–Managed Keys \(SSE\-KMS\)](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/encrypting-cloudtrail-log-files-with-aws-kms.html) in the *AWS CloudTrail User Guide*\.
+ [Use the targets and rate control features in Run Command to perform a staged command operation](send-commands-multiple.md)\.
+ [Use fine\-grained access permissions for Run Command \(and all Systems Manager capabilities\) by using AWS Identity and Access Management \(IAM\) policies](security_iam_id-based-policy-examples.md#customer-managed-policies)\.

**[Session Manager](session-manager.md)**
+ [Audit session activity in your AWS account using AWS CloudTrail](session-manager-auditing.md)\.
+ [Log session data in your AWS account using Amazon CloudWatch Logs or Amazon S3](session-manager-logging.md)\.
+ [Control user session access to instances](session-manager-getting-started-restrict-access.md)\.
+ [Restrict access to commands in a session](session-manager-restrict-command-access.md)\.
+ [Turn off or turn on ssm\-user account administrative permissions](session-manager-getting-started-ssm-user-permissions.md)\.

**[State Manager](systems-manager-state.md)**
+ [Update SSM Agent at least once a month using the pre\-configured `AWS-UpdateSSMAgent` document](sysman-state-cli.md)\.
+ \(Windows\) Upload the PowerShell or DSC module to Amazon Simple Storage Service \(Amazon S3\), and use `AWS-InstallPowerShellModule`\.
+ Use Amazon Elastic Compute Cloud \(Amazon EC2\) tags to create application groups for your instances\. And then target instances using the `Targets` parameter instead of specifying individual instance IDs\.
+ [Automatically remediate findings generated by Amazon Inspector by using Systems Manager](http://aws.amazon.com/blogs/security/how-to-remediate-amazon-inspector-security-findings-automatically/)\.
+ [Use a centralized configuration repository for your SSM documents, and share documents across your organization](ssm-sharing.md)\.
+ For information about the differences between State Manager and Maintenance Windows, see [Choosing between State Manager and Maintenance Windows](state-manager-vs-maintenance-windows.md)\.

**[Managed Instances](managed_instances.md)**
+ Systems Manager requires accurate time references to perform its operations\. If your instance's date and time aren't set correctly, they might not match the signature date of your API requests\. This might lead to errors or incomplete functionality\. For example, instances with incorrect time settings won't be included in your lists of managed instances\.

  For information about setting the time on your instances, see the following topics: 
  +  [Set the time for your Linux instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/set-time.html)
  +  [Set the time for a Windows instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/windows-set-time.html)