# AWSSupport\-CollectEKSInstanceLogs<a name="automation-awssupport-collecteksinstancelogs"></a>

**Description**

The AWSSupport\-CollectEKSInstanceLogs runbook gathers operating system and Amazon Elastic Kubernetes Service \(Amazon EKS\) related log files from an Amazon Elastic Compute Cloud \(Amazon EC2\) instance to help you troubleshoot common issues\. While the automation is gathering the associated log files, changes are made to the file system structure including the creation of temporary directories, the copying of log files to the temporary directories, and compressing the log files into an archive\. This activity can result in increased `CPUUtilization` on the EC2 instance\. For more information about `CPUUtilization`, see [Instance metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/viewing_metrics_with_cloudwatch.html#ec2-cloudwatch-metrics) in the *Amazon CloudWatch User Guide*\.

If you specify a value for the `LogDestination` parameter, the automation evaluates the policy status of the Amazon Simple Storage Service \(Amazon S3\) bucket you specify\. To help with the security of the logs gathered from your EC2 instance, if the policy status `isPublic` is set to `true`, or if the access control list \(ACL\) grants `READ|WRITE` permissions to the `All Users` Amazon S3 predefined group, the logs are not uploaded\. For more information about Amazon S3 predefined groups, see [ Amazon S3 predefined groups](https://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html#specifying-grantee-predefined-groups) in the *Amazon Simple Storage Service Developer Guide*\.

**Note**  
This automation requires at least 10 percent of available disk space on the root Amazon Elastic Block Store \(Amazon EBS\) volume attached to your EC2 instance\. If there is not enough available disk space on the root volume, the automation stops\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-CollectEKSInstanceLogs)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ EKSInstanceId

  Type: String

  Description: \(Required\) ID of the Amazon EKS EC2 instance you want to collect logs from\.
+ LogDestination

  Type: String

  Description: \(Optional\) The S3 bucket in your account to upload the archived logs to\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:ExecuteAutomation`
+ `ssm:GetAutomationExecution`
+ `ssm:SendCommand`

We recommend that the EC2 instance receiving the command has an IAM role with the **AmazonSSMManagedInstanceCore** Amazon managed policy attached\. To upload the log archive to the S3 bucket you specify in the `LogDestination` parameter, you must add the `s3:PutObject` permission\.

**Document Steps**
+ aws:assertAwsResourceProperty \- Confirms the operating system of the value specified in the `EKSInstanceId` parameter is Linux\.
+ aws:runCommand \- Gathers operating system and Amazon EKS related log files, compressing them into an archive in the `/var/log` directory\.
+ aws:branch \- Confirms whether a value was specified for the `LogDestination` parameter\.
+ aws:runCommand \- Uploads the log archive to the S3 bucket you specify in the `LogDestination` parameter\.