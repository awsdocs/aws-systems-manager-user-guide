# Amazon EventBridge target examples for Systems Manager<a name="monitoring-systems-manager-targets"></a>

When you specify the target to invoke in an Amazon EventBridge rule, you can choose from over 20 target types and add up to five targets to each rule\.

Of the various targets, you can choose from Automation, OpsCenter, and Run Command, which are capabilities of AWS Systems Manager, as target actions when an EventBridge event occurs\.

The following are several examples of ways you can use these capabilities as the target of an EventBridge rule\.

**Automation examples**  
You can configure an EventBridge rule to start Automation workflows when events such as the following occur:
+ When an Amazon CloudWatch alarm reports that a managed instance has failed a status check \(`StatusCheckFailed_Instance=1`\), run the `AWSSupport-ExecuteEC2Rescue` Automation runbook on the instance\.
+ When an `EC2 Instance State-change Notification` event occurs because a new Amazon Elastic Compute Cloud \(Amazon EC2\) instance is running, run the `AWS-AttachEBSVolume` Automation runbook on the instance\.
+ When an Amazon Elastic Block Store \(Amazon EBS\) volume is created and available, run the `AWS-CreateSnapshot` Automation runbook on the volume\.

**OpsCenter examples**  
You can configure an EventBridge rule to create a new OpsItem when incidents such as the following occur:
+ A throttling event for Amazon DynamoDB occurs, or Amazon EBS volume performance has degraded\.
+ An Amazon EC2 Auto Scaling group fails to launch an instance, or a Systems Manager Automation workflow fails\.
+ An EC2 instance changes state from `Running` to `Stopped`\.

**Run Command examples**  
You can configure an EventBridge rule to run a Systems Manager Command document in Run Command when events such as the following occur:
+ When an Auto Scaling group is about to end, a Run Command script could capture the log files from the instance before it is ended\.
+ When a new instance is created in an Auto Scaling group, a Run Command target action could turn on the web server role or install software on the instance\.
+ When a managed instance is found to be out of compliance, a Run Command target action could update patches on the instance by running the `AWS-RunPatchBaseline` document\.