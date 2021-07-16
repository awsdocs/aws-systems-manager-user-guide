# AWS Systems Manager OpsCenter<a name="OpsCenter"></a>

OpsCenter, a capability of AWS Systems Manager, provides a central location where operations engineers and IT professionals can view, investigate, and resolve operational work items \(OpsItems\) related to AWS resources\. OpsCenter is designed to reduce mean time to resolution for issues impacting AWS resources\. This Systems Manager capability aggregates and standardizes OpsItems across services while providing contextual investigation data about each OpsItem, related OpsItems, and related resources\. OpsCenter also provides Systems Manager Automation runbooks that you can use to quickly resolve issues\. You can specify searchable, custom data for each OpsItem\. You can also view automatically\-generated summary reports about OpsItems by status and source\. 

OpsCenter is integrated with Amazon EventBridge and Amazon CloudWatch\. This means you can configure these services to automatically create an OpsItem in OpsCenter when a CloudWatch alarm enters the `ALARM` state or when EventBridge processes an event from any AWS service that publishes events\. Configuring CloudWatch alarms and EventBridge events to automatically create OpsItems allows you to quickly diagnose and remediate issues with AWS resources from a single console\.

To help you diagnose issues, each OpsItem includes contextually relevant information such as the name and ID of the AWS resource that generated the OpsItem, alarm or event details, alarm history, and an alarm timeline graph\.

For the AWS resource, OpsCenter aggregates information from AWS Config, AWS CloudTrail logs, and Amazon CloudWatch Events, so you don't have to navigate across multiple console pages during your investigation\.

The following list includes types of AWS resources and metrics for which customers configure CloudWatch alarms that create OpsItems\.
+ Amazon DynamoDB: database read and write actions reach a threshold
+ Amazon EC2: CPU utilization reaches a threshold
+ AWS billing: estimated charges reach a threshold
+ Amazon EC2: an instance fails a status check
+ Amazon Elastic Block Store \(EBS\): disk space utilization reaches a threshold

The following list includes types of EventBridge rules customer configure to create OpsItems\.
+ AWS Security Hub: security alert issued
+ DynamoDB: a throttling event
+ Amazon EC2 Auto Scaling: failure to launch an instance
+ Systems Manager: failure to run an automation
+ AWS Health: an alert for scheduled maintenance
+ EC2: instance state change from `Running` to `Stopped`

OpsCenter is also integrated with Amazon CloudWatch Application Insights for \.NET and SQL Server\. This means you can automatically create OpsItems for problems detected in your applications\. You can also integrate OpsCenter with AWS Security Hub to aggregate and take action on your security, performance, and operational issues in Systems Manager\. 

Operations engineers and IT professionals can create, view, and edit OpsItems by using the OpsCenter page in the AWS Systems Manager console, public API operations, the AWS Command Line Interface \(AWS CLI\), AWS Tools for Windows PowerShell, or the AWS SDKs\. OpsCenter public API operations also allows you to integrate OpsCenter with your case management systems and health dashboards\. 

## OpsCenter integration<a name="OpsCenter-integration"></a>

The following table describes how OpsCenter integrates with other AWS services and Systems Manager capabilities\. When it's integrated with these services and capabilities, OpsCenter helps you to quickly diagnose and remediate issues with AWS resources from a single console\. 


****  

| Service or capability | Details | For more information | 
| --- | --- | --- | 
|  EventBridge  |  You can configure Amazon EventBridge to automatically create an OpsItem in OpsCenter when the system processes an event from any AWS service that publishes events\. The following list includes types of EventBridge rules that you can configure to create OpsItems: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter.html) To help you diagnose issues, each OpsItem includes contextually relevant information about the event, such as the name and ID of the AWS resource that generated the OpsItem and details about the event\.  | [Configuring EventBridge to automatically create OpsItems for specific events](OpsCenter-automatically-create-OpsItems-2.md) | 
|  CloudWatch  |  You can configure Amazon CloudWatch to automatically create an OpsItem in OpsCenter when a CloudWatch alarm enters the `ALARM` state\. The following list includes types of AWS resources and metrics for which you can configure CloudWatch alarms to create OpsItems: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter.html) To help you diagnose issues, each OpsItem includes contextually relevant information about the alarm, such as the name and ID of the AWS resource that generated the OpsItem, alarm details, alarm history, and an alarm timeline graph\.  | [Configuring CloudWatch to create OpsItems from alarms](OpsCenter-create-OpsItems-from-CloudWatch-Alarms.md) | 
|  Incident Manager  |  AWS Incident Manager, a capability of Systems Manager, provides an incident management console that helps you mitigate and recover from incidents affecting your AWS hosted applications\. An *incident* is any unplanned interruption or reduction in quality of services\. After you set up and configure Incident Manager, the system automatically creates OpsItems in OpsCenter when incidents are created in Incident Manager\. You can also manually add incidents to an OpsItem\. After an incident is resolved, post incident analysis guides you through identifying improvements to your incident response and recommending action items to address the findings\. With high severity operational issues such as incidents, creating an OpsItem in OpsCenter provides operators with a complete view of the incident, analysis, and action items\. This comprehensive view improves time\-to\-resolution and helps mitigate similar issues in the future\.  |  [Working with Incident Manager incidents in OpsCenter](OpsCenter-create-OpsItems-for-Incident-Manager.md) [AWS Systems Manager Incident Manager User Guide](https://docs.aws.amazon.com/incident-manager/latest/userguide)  | 
|  CloudWatch Application Insights for \.NET and SQL Server\.  |  OpsCenter is also integrated with CloudWatch Application Insights for \.NET and SQL Server\. CloudWatch Application Insights helps you monitor your applications that use Amazon EC2 instances along with other application resources\. This capability identifies and sets up key metrics, logs, and alarms across your application resources and technology stack\. This capability also creates automated dashboards for detected problems\. Dashboards include correlated metric anomalies, log errors, and other information to help you determine the root cause of the errors\. When you configure application resources in CloudWatch Application Insights, you can choose to have the system create OpsItems in OpsCenter when problems are detected\.  |  [Setting Up Your Application](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/appinsights-setting-up.html) in the *Amazon CloudWatch User Guide*  | 

For each AWS resource that automatically generates an OpsItem, OpsCenter aggregates information from AWS Config, AWS CloudTrail logs, and EventBridge\. As a result, you don't have to navigate across multiple console pages during your investigation\.

## How can OpsCenter benefit my organization?<a name="OpsCenter-learn-more-benefits"></a>

OpsCenter provides a standard and unified experience for viewing, working on, and remediating issues related to AWS resources\. A standard and unified experience improves the time it takes to remedy issues, investigate related issues, and train new operations engineers and IT professionals\. A standard and unified experience also reduces the number of manual errors entered into the system of managing and remediating issues\. 

More specifically, OpsCenter offers the following benefits for operations engineers and organizations:
+ You no longer need to navigate across multiple console pages to view, investigate, and resolve OpsItems related to AWS resources\. OpsItems are aggregated, across services, in a central location\.
+ You can view service\-specific and contextually relevant data for OpsItems that are automatically generated by CloudWatch alarms, EventBridge events, and CloudWatch Application Insights for \.NET and SQL Server\.
+ You can specify the Amazon Resource Name \(ARN\) of a resource related to an OpsItem\. By specifying related resources, OpsCenter uses built\-in logic to help you avoid creating duplicate OpsItems\.
+ You can view details and resolution information about similar OpsItems\.
+ You can quickly view information about and run Systems Manager Automation runbooks to resolve issues\.

## What are the features of OpsCenter?<a name="OpsCenter-learn-more-features"></a>
+ **Automated and manual OpsItem creation**

  OpsCenter is integrated with Amazon CloudWatch\. This means you can configure CloudWatch to automatically create an OpsItem in OpsCenter when an alarm enters the `ALARM` state or when Amazon EventBridge processes an event from any AWS service that publishes events\. You can also manually create OpsItems\.

  OpsCenter is also integrated with Amazon CloudWatch Application Insights for \.NET and SQL Server\. This means you can automatically create OpsItems for problems detected in your applications\.
+ **Detailed and searchable OpsItems**

  Each OpsItem includes multiple fields of information, including a title, ID, priority, description, the source of the OpsItem, and the date/time it was last updated\. Each OpsItem also includes the following configurable features:
  + **Status**: Open, In progress, Resolved, or Open and In progress\.
  + **Related resources**: A related resource is the impacted resource or the resource that initiated the EventBridge event that created the OpsItem\. Each OpsItem includes a **Related resources** section where OpsCenter automatically lists the Amazon Resource Name \(ARN\) of the related resource\. You can also manually specify ARNs of related resources\. For some ARN types, OpsCenter automatically creates a deep link that displays details about the resource without having to visit other console pages to view that information\. For example, if you specify the ARN of an EC2 instance, you can view all of the EC2\-provided details about that instance in OpsCenter\. You can manually add the ARNs of additional related resources\. Each OpsItem can list a maximum of 100 related resource ARNs\. For more information, see [Working with related resources](OpsCenter-working-with-OpsItems.md#OpsCenter-working-with-OpsItems-related-resources)\.
  + **Related and Similar OpsItems**: With the **Related OpsItems** feature, you can specify the IDs of OpsItems that are in some way related to the current OpsItem\. The **Similar OpsItem** feature automatically reviews OpsItem titles and descriptions and then lists other OpsItems that might be related or of interest to you\.
  + **Searchable and private operational data**: Operational data is custom data that provides useful reference details about the OpsItem\. For example, you can specify log files, error strings, license keys, troubleshooting tips, or other relevant data\. You enter operational data as key\-value pairs\. The key has a maximum length of 128 characters\. The value has a maximum size of 20 KB\.

    This custom data is searchable, but with restrictions\. For the **Searchable operational data** feature, all users with access to the OpsItem Overview page \(as provided by the [DescribeOpsItems](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeOpsItems.html) API operation\) can view and search on the specified data\. For the **Private operational data** feature, the data is only viewable by users who have access to the OpsItem \(as provided by the [GetOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetOpsItem.html) API operation\)\.
  + **Deduplication**: By specifying related resources, OpsCenter uses built\-in logic to help you avoid creating duplicate OpsItems\. OpsCenter also includes a feature called **Operational insights**, which displays information about duplicate OpsItems\. To further limit the number of duplicate OpsItems in your account, you can manually specify a deduplication string for an EventBridge event rule\. For more information, see [Reducing duplicate OpsItems](OpsCenter-working-deduplication.md)\. 
+ **Easy remediation using runbooks**

  Each OpsItem includes a **Runbooks** section with a list of Systems Manager Automation runbooks that you can use to automatically remediate common issues with AWS resources\. If you open an OpsItem, choose an AWS resource for that OpsItem, and then choose the **Run automation** button in the console, then OpsCenter provides a list of Automation runbooks that you can run on the AWS resource that generated the OpsItem\. After you run an Automation runbook from an OpsItem, the runbook is automatically associated with the related resource of the OpsItem for future reference\. Additionally, if you automatically set up OpsItem rules in EventBridge by using OpsCenter, then EventBridge automatically associates runbooks for common events\. OpsCenter keeps a 30\-day record of Automation runbooks run for a specific OpsItem\. For more information, see [Remediating OpsItem issues using Systems Manager Automation](OpsCenter-remediating.md)\.
+ **Change notification**: You can specify the ARN of an Amazon Simple Notification Service \(SNS\) topic and publish notifications anytime an OpsItem is changed or edited\. The SNS topic must exist in the same AWS Region as the OpsItem\.
+ **Comprehensive OpsItem search capabilities**: OpsCenter provides multiple search options to help you quickly locate OpsItems\. Here are several examples of how you can search: OpsItem ID, Title, Last modified time, Operational data value, Source, and Automation ID of a runbook execution, to name a few\. You can further limit search results by using status filters\. 
+ **OpsItem summary reports**

  OpsCenter includes a summary report page that automatically displays the following sections:
  + **Status summary**: a summary of OpsItems by status \(Open, In progress, Resolved, Open and In progress\)\.
  + **Sources with most open OpsItems**: a breakdown of the top AWS services with open OpsItems\.
  + **OpsItems by source and age**: a count of OpsItems grouped by source and days since creation\.

  For more information about viewing OpsCenter summary reports, see [Viewing OpsCenter summary reports](OpsCenter-reports.md)\.
+ **IAM access control**

  By using AWS Identity and Access Management \(IAM\) policies, you can control which members of your organization can create, view, list, and update OpsItems\. You can also assign tags to OpsItems and then create IAM policies that give access to users and groups based on tags\. For more information, see [Getting started with OpsCenter](OpsCenter-getting-started.md)\.
+ **Logging and auditing capability support**

  You can audit and log OpsCenter user actions in your AWS account through integration with other AWS services\. For more information, see [Auditing and logging OpsCenter activity](OpsCenter-logging-auditing.md)\.
+ **Console, CLI, PowerShell, and SDK access to OpsCenter capabilities**

  You can work with OpsCenter by using the AWS Systems Manager console, AWS Command Line Interface \(AWS CLI\), AWS Tools for PowerShell, or the AWS SDK of your choice\.

## How does OpsCenter work with Amazon EventBridge? Which service should I use?<a name="OpsCenter-learn-more-compare-CWE"></a>

Amazon EventBridge delivers a near real\-time stream of system events that describe changes in AWS resources\. Using simple rules that you can quickly set up, you can match events and route them to one or more target functions or streams\. Generally speaking, EventBridge informs you that there is a problem with your resources\.

OpsCenter helps you investigate and remediate the problem\. OpsCenter brings together data from EventBridge or data entered manually by engineers so that your engineers can perform a thorough investigation\. OpsCenter also provides Automation runbooks for quickly remediating those issues\. OpsCenter integrates with EventBridge by allowing you to automatically create OpsItems \(or you can manually create OpsItems\) to address the following types of issues: performance degradation, state changes, execution failures, maintenance notifications, and security alerts\.

## Does OpsCenter integrate with my existing case management system?<a name="OpsCenter-learn-more-case-management"></a>

OpsCenter is designed to complement your existing case management systems\. You can integrate OpsItems into your existing case management system by using public API operations\. You can also maintain manual lifecycle workflows in your current systems and use OpsCenter as an investigation and remediation hub\. 

For information about OpsCenter public API operations, see the following API operations in the *AWS Systems Manager API Reference*\.
+ [CreateOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateOpsItem.html)
+ [DescribeOpsItems](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeOpsItems.html)
+ [GetOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetOpsItem.html)
+ [GetOpsSummary](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetOpsSummary.html)
+ [UpdateOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateOpsItem.html)

## Is there a charge to use OpsCenter?<a name="OpsCenter-learn-more-cost"></a>

Yes\. For more information, see [AWS Systems Manager Pricing](http://aws.amazon.com/systems-manager/pricing/)\.

## Does OpsCenter work with my on\-premises and hybrid managed instances?<a name="OpsCenter-learn-more-hybrid"></a>

Yes\. You can use OpsCenter to investigate and remediate issues with your on\-premises managed instances that are configured for Systems Manager\. For more information about setting up and configuring on\-premises servers and virtual machines for Systems Manager, see [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)\.

## What are the quotas for OpsCenter?<a name="OpsCenter-learn-more-limits"></a>

You can view quotas for all Systems Manager capabilities in the [Systems Manager service quotas](https://docs.aws.amazon.com/general/latest/gr/ssm.html#limits_ssm) in the *Amazon Web Services General Reference*\. Unless otherwise noted, each quota is Region\-specific\.

**Topics**
+ [OpsCenter integration](#OpsCenter-integration)
+ [How can OpsCenter benefit my organization?](#OpsCenter-learn-more-benefits)
+ [What are the features of OpsCenter?](#OpsCenter-learn-more-features)
+ [How does OpsCenter work with Amazon EventBridge? Which service should I use?](#OpsCenter-learn-more-compare-CWE)
+ [Does OpsCenter integrate with my existing case management system?](#OpsCenter-learn-more-case-management)
+ [Is there a charge to use OpsCenter?](#OpsCenter-learn-more-cost)
+ [Does OpsCenter work with my on\-premises and hybrid managed instances?](#OpsCenter-learn-more-hybrid)
+ [What are the quotas for OpsCenter?](#OpsCenter-learn-more-limits)
+ [Getting started with OpsCenter](OpsCenter-getting-started.md)
+ [Creating OpsItems](OpsCenter-creating-OpsItems.md)
+ [Working with OpsItems](OpsCenter-working-with-OpsItems.md)
+ [Reducing duplicate OpsItems](OpsCenter-working-deduplication.md)
+ [Working with Incident Manager incidents in OpsCenter](OpsCenter-create-OpsItems-for-Incident-Manager.md)
+ [Remediating OpsItem issues using Systems Manager Automation](OpsCenter-remediating.md)
+ [Viewing OpsCenter summary reports](OpsCenter-reports.md)
+ [Supported resources reference](OpsCenter-related-resources-reference.md)
+ [Receiving findings from AWS Security Hub in OpsCenter](opscenter-securityhub-integration.md)
+ [Auditing and logging OpsCenter activity](OpsCenter-logging-auditing.md)