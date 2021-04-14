# Systems Manager Automation runbook reference<a name="automation-documents-reference"></a>

To help you get started quickly, AWS Systems Manager provides predefined runbooks\. These runbooks are maintained by Amazon Web Services, AWS Support, and AWS Config\. The runbook reference describes each of the predefined runbooks provided by Systems Manager, AWS Support, and AWS Config\.

**Important**  
If you run an automation workflow that invokes other services by using an AWS Identity and Access Management \(IAM\) service role, be aware that the service role must be configured with permission to invoke those services\. This requirement applies to all AWS Automation runbooks \(`AWS-*` runbooks\) such as the `AWS-ConfigureS3BucketLogging`, `AWS-CreateDynamoDBBackup`, and `AWS-RestartEC2Instance` runbooks, to name a few\. This requirement also applies to any custom Automation runbooks you create that invoke other AWS services by using actions that call other services\. For example, if you use the `aws:executeAwsApi`, `aws:createStack`, or `aws:copyImage` actions, then you must configure the service role with permission to invoke those services\. You can enable permissions to other AWS services by adding an IAM inline policy to the role\. For more information, see [\(Optional\) Add an Automation inline policy to invoke other AWS services](automation-permissions.md#automation-role-add-inline-policy)\.

This reference includes topics that describe each of the Systems Manager runbooks that are owned by AWS, AWS Support, and AWS Config\. Runbooks are organized by the relevant AWS service\. Each page provides an explanation of the required and optional parameters you can specify when using the runbook\. Each page also lists the steps in the runbook and the output of the automation, if any\. 

This section does *not* include a separate page for runbooks that require approval such as the AWS\-CreateManagedLinuxInstanceWithApproval or AWS\-StopEC2InstanceWithApproval runbook\. Any runbook name that includes *WithApproval*, means the runbook includes the [aws:approve – Pause an automation for manual approval](automation-action-approve.md) action\. This action temporarily pauses an automation until designated principals either approve or reject the action\. After the required number of approvals is reached, the automation resumes\. 

For information about running automations, see [Running a simple automation](automation-working-executing.md)\. For information about running automations on multiple targets, see [Running automations that use targets and rate controls](automation-working-targets-and-rate-controls.md)\.

## View runbook content<a name="view-automation-json"></a>

You can view the content for runbooks in the Systems Manager console\.

**To view runbook content**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose a runbook, and then choose **View details**\.

1. Choose the **Content** tab\.

**Topics**
+ [View runbook content](#view-automation-json)
+ [API Gateway](automation-ref-abp.md)
+ [AWS CloudFormation](automation-ref-cfn.md)
+ [CloudFront](automation-ref-cf.md)
+ [CloudTrail](automation-ref-ct.md)
+ [CloudWatch](automation-ref-cw.md)
+ [CodeBuild](automation-ref-acb.md)
+ [AWS Directory Service](automation-ref-ads.md)
+ [DynamoDB](automation-ref-ddb.md)
+ [Amazon EBS](automation-ref-ebs.md)
+ [Amazon EC2](automation-ref-ec2.md)
+ [Amazon EFS](automation-ref-efs.md)
+ [Amazon EKS](automation-ref-eks.md)
+ [Elastic Beanstalk](automation-ref-aeb.md)
+ [Elastic Load Balancing](automation-ref-elb.md)
+ [Amazon ES](automation-ref-es.md)
+ [GuardDuty](automation-ref-gdu.md)
+ [IAM](automation-ref-iam.md)
+ [AWS KMS](automation-ref-kms.md)
+ [Lambda](automation-ref-lam.md)
+ [Amazon RDS](automation-ref-rds.md)
+ [Amazon Redshift](automation-ref-rs.md)
+ [Amazon S3](automation-ref-s3.md)
+ [Secrets Manager](automation-ref-asm.md)
+ [Security Hub](automation-ref-ash.md)
+ [Amazon SNS](automation-ref-sns.md)
+ [Systems Manager](automation-ref-sys.md)
+ [Third\-party](automation-ref-third-party.md)
+ [Amazon VPC](automation-ref-vpc.md)
+ [AWS WAF](automation-ref-waf.md)
+ [Amazon WorkSpaces](automation-ref-wsp.md)
+ [X\-Ray](automation-ref-xray.md)