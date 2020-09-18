# AWS Systems Manager Automation<a name="systems-manager-automation"></a>

Systems Manager Automation simplifies common maintenance and deployment tasks of EC2 instances and other AWS resources\. Automation enables you to do the following:
+ Build Automation workflows to configure and manage instances and AWS resources\.
+ Create custom workflows or use pre\-defined workflows maintained by AWS\.
+ Receive notifications about Automation tasks and workflows by using Amazon EventBridge\.
+ Monitor Automation progress and execution details by using the Amazon EC2 or the AWS Systems Manager console\. 

**Primary Components**  
AWS Systems Manager Automation uses the following components to run *automation workflows*\.


****  

| Concept | Details | 
| --- | --- | 
|  Automation document  |  A Systems Manager Automation document defines the Automation workflow \(the actions that Systems Manager performs on your managed instances and AWS resources\)\. Automation includes several pre\-defined Automation documents that you can use to perform common tasks like restarting one or more EC2 instances or creating an Amazon Machine Image \(AMI\)\. You can create your own Automation documents as well\. Documents use JavaScript Object Notation \(JSON\) or YAML, and they include steps and parameters that you specify\. Steps run in sequential order\. For more information, see [Working with Automation documents](automation-documents.md)\. Automation documents are Systems Manager documents of type `Automation`, as opposed to `Command`, `Policy`, `Session` documents\. Automation documents currently support schema version 0\.3\. Command documents use schema version 1\.2, 2\.0, or 2\.2\. Policy documents use schema version 2\.0 or later\.  | 
|  Automation action  |  The Automation workflow defined in an Automation document includes one or more steps\. Each step is associated with a particular action\. The action determines the inputs, behavior, and outputs of the step\. Steps are defined in the `mainSteps` section of your Automation document\. Automation supports 20 distinct action types\. For more information, see the [Systems Manager Automation actions reference](automation-actions.md)\.  | 
|  Automation quota  |  Each AWS account can run 100 automations simultaneously\. This includes child automations \(automations that are started by another automation\), and rate control automations\. If you attempt to run more automations than this, Systems Manager adds the additional automations to a queue and displays a status of Pending\. For more information on running automations, see [Running a simple automation](automation-working-executing.md)\.  | 
|  Automation queue quota  |  If you attempt to run more automations than the concurrent automation limit, subsequent automations are added to a queue\. Each AWS account can queue 1,000 automations\. When an automation completes \(or reaches a terminal state\), the first automation in the queue is started\.  | 
|  Rate control automation quota  |  Each AWS account can run 25 rate control automations simultaneously\. If you attempt to run more rate control automations than the concurrent rate control automation limit, Systems Manager adds the subsequent rate control automations to a queue and displays a status of Pending\. For more information on running rate control automations, see [Running automations that use targets and rate controls](automation-working-targets-and-rate-controls.md)\.  | 
|  Rate control automation queue quota  |  If you attempt to run more automations than the concurrent rate control automation limit, subsequent automations are added to a queue\. Each AWS account can queue 1,000 rate control automations\. When an automation completes \(or reaches a terminal state\), the first automation in the queue is started\.  | 

## Automation use cases<a name="automation-use-cases"></a>

This section includes common uses cases for AWS Systems Manager Automation\.

**Perform common IT tasks**  
Automation can simplify common IT tasks such as changing the state of one or more instances \(using an approval workflow\) and managing instance states according to a schedule\. Here are some examples:
+ Use the AWS\-StopEC2InstanceWithApproval document to request that one or more AWS Identity and Access Management \(IAM\) users approve the instance stop action\. After the approval is received, Automation stops the instance\.
+ Use the AWS\-StopEC2Instance document to automatically stop instances on a schedule by using Amazon EventBridge or by using a maintenance window task\. For example, you can configure an Automation workflow to stop instances every Friday evening, and then restart them every Monday morning\.
+ Use the AWS\-UpdateCloudFormationStackWithApproval document to update resources that were deployed by using CloudFormation template\. The update applies a new template\. You can configure the Automation to request approval by one or more IAM users before the update begins\.

For information about how to run an Automation workflow by using State Manager, see [Running automations with triggers using State Manager](automation-sm-target.md)\.

**Safely perform disruptive tasks in bulk**  
Systems Manager includes features that help you target large groups of instances by using Amazon EC2 tags, and velocity controls that help you roll out changes according to the limits you define\.

Use the AWS\-RestartEC2InstanceWithApproval document to target an AWS resource group that includes multiple instances\. You can configure the Automation workflow to use velocity controls\. For example, you can specify the number of instances that should be restarted concurrently\. You can also specify a maximum number of errors that are allowed before the Automation workflow is cancelled\.

**Simplify complex tasks**  
Automation offers one\-click automations for simplifying complex tasks such as creating golden Amazon Machines Images \(AMIs\), and recovering unreachable EC2 instances\. Here are some examples:
+ Use the `AWS-UpdateLinuxAmi` and `AWS-UpdateWindowsAmi` documents to create golden AMIs from a source AMI\. You can run custom scripts before and after updates are applied\. You can also include or exclude specific packages from being installed\. For examples of how to run these workflows, see [Automation walkthroughs](automation-walk.md)\.
+ Use the AWSSupport\-ExecuteEC2Rescue document to recover impaired instances\. An instance can become unreachable for a variety of reasons, including network misconfigurations, RDP issues, or firewall settings\. Troubleshooting and regaining access to the instance previously required dozens of manual steps before you could regain access\. The AWSSupport\-ExecuteEC2Rescue document lets you regain access by specifying an instance ID and clicking a button\. For an example of how to run this workflow, see [Walkthrough: Run the EC2Rescue tool on unreachable instances](automation-ec2rescue.md)\.

**Enhance operations security**  
Using delegated administration, you can restrict or elevate user permissions for various types of tasks\. 

Delegated administration enables you to provide permissions for certain tasks on certain resource without having to give a user direct permission to access the resources\. This improves your overall security profile\. For example, assume that User1 doesn’t have permissions to restart EC2 instances, but you would like to authorize the user to do so\. Instead of allowing User1 direct permissions, you can: 
+ Create an IAM role with the permissions required to successfully stop and start EC2 instances\.
+ Create an Automation document and embed the role in the document\. \(The easiest way to do this is to customize the AWS\-RestartEC2Instance document and embed the role in the document instead of assigning an Automation service role \[or *assume role*\]\)\.
+ Modify IAM permissions for User1 and allow the user permission to run the document\. 

For an example of how to delegate access to an Automation workflow, see [Running an automation by using delegated administration](automation-walk-security-delegated.md)\. 

**Share best practices**  
Automation lets you share best practices with rest of your organization\.

You can create best practices for resource management in Automation documents and easily share the documents across AWS Regions and groups\. You can also constrain the allowed values for the parameters the document accepts\.

**Amazon EventBridge support**  
This Systems Manager capability is supported as a *target* type in EventBridge rules\. For information, see [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md) and [Reference: Amazon EventBridge event patterns and types for Systems Manager](reference-eventbridge-events.md)\.

**Topics**
+ [Automation use cases](#automation-use-cases)
+ [Getting started with Automation](automation-setup.md)
+ [Working with automations](automation-working.md)
+ [Systems Manager Automation actions reference](automation-actions.md)
+ [Working with Automation documents](automation-documents.md)
+ [Systems Manager Automation documents reference](automation-documents-reference.md)
+ [Automation walkthroughs](automation-walk.md)
+ [Troubleshooting Systems Manager Automation](automation-troubleshooting.md)