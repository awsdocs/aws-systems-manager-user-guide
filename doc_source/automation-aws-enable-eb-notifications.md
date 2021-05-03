# AWSConfigRemediation\-EnableBeanstalkEnvironmentNotifications<a name="automation-aws-enable-eb-notifications"></a>

**Description**

The AWSConfigRemediation\-EnableBeanstalkEnvironmentNotifications runbook enables notifications for the AWS Elastic Beanstalk \(Elastic Beanstalk\) environment you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableBeanstalkEnvironmentNotifications)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ EnvironmentId

  Type: String

  Description: \(Required\) The ID of the Elastic Beanstalk environment that you want to enable notifications for\.
+ TopicArn

  Type: String

  Description: \(Required\) The ARN of the Amazon Simple Notification Service \(Amazon SNS\) topic you want to send notifications to\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `elasticbeanstalk:DescribeConfigurationSettings`
+ `elasticbeanstalk:DescribeEnvironments`
+ `elasticbeanstalk:UpdateEnvironment`

**Document Steps**
+ aws:executeAwsApi \- Enables notifications for the Elastic Beanstalk environment you specify in the `EnvironmentId` parameter\.
+ aws:waitForAwsResourceProperty \- Waits for the status of the environment to change to `Ready`\.
+ aws:executeScript \- Verifies notifications have been enabled for the Elastic Beanstalk environment\.