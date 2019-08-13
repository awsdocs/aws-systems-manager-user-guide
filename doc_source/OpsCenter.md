# AWS Systems Manager OpsCenter<a name="OpsCenter"></a>

OpsCenter provides a central location where operations engineers and IT professionals can view, investigate, and resolve operational work items \(OpsItems\) related to AWS resources\. OpsCenter is designed to reduce mean time to resolution for issues impacting AWS resources\. This Systems Manager capability aggregates and standardizes OpsItems across services while providing contextual investigation data about each OpsItem, related OpsItems, and related resources\. OpsCenter also provides Systems Manager Automation documents \(runbooks\) that you can use to quickly resolve issues\. You can specify searchable, custom data for each OpsItem\. You can also view automatically\-generated summary reports about OpsItems by status and source\. 

OpsCenter is integrated with Amazon CloudWatch Events\. This means you can create CloudWatch Events rules that automatically create OpsItems for any AWS service that publishes events to CloudWatch Events\. For example, you can configure **SSM OpsItems** as the target for the following types of events, and hundreds more:
+ Security issues, such as alerts from AWS Security Hub
+ Performance issues, such as a throttling event for Amazon DynamoDB or degraded Amazon Elastic Block Store \(EBS\) volume performance
+ Failures, such as an Amazon EC2 Auto Scaling group failure to launch an instance or a Systems Manager Automation execution failure
+ Health alerts, such as an AWS Health alert for scheduled maintenance
+ State changes, such as an Amazon EC2 instance state change from `Running` to `Stopped`

OpsCenter is also integrated with Amazon CloudWatch Application Insights for \.NET and SQL Server\. This means you can automatically create OpsItems for problems detected in your applications\.

Operations engineers and IT professionals can create, view, and edit OpsItems by using the OpsCenter page in the AWS Systems Manager console, public API actions, the AWS CLI, AWS Tools for Windows PowerShell, or the AWS SDKs\. You can also use AWS Lambda with Amazon SNS to create OpsItems from sources like CloudWatch alarms\. OpsCenter public API actions also enable you to integrate OpsCenter with your case management systems and health dashboards\. 

**Topics**
+ [Learn More About OpsCenter](OpsCenter-learn-more.md)
+ [Getting Started with OpsCenter](OpsCenter-getting-started.md)
+ [Creating OpsItems](OpsCenter-creating-OpsItems.md)
+ [Working with OpsItems](OpsCenter-working-with-OpsItems.md)
+ [Remediating OpsItem Issues Using Systems Manager Automation](OpsCenter-remediating.md)
+ [Viewing OpsCenter Summary Reports](OpsCenter-reports.md)
+ [Supported Resources Reference](OpsCenter-related-resources-reference.md)
+ [Auditing and Logging OpsCenter Activity](OpsCenter-logging-auditing.md)