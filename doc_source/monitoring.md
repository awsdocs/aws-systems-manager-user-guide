# Monitoring AWS Systems Manager<a name="monitoring"></a>

Monitoring is an important part of maintaining the reliability, availability, and performance of Systems Manager and your AWS solutions\. You should collect monitoring data from all of the parts of your AWS solution so that you can more easily debug a multipoint failure if one occurs\. But before you start monitoring Systems Manager, you should create a monitoring plan that includes answers to the following questions: 
+ What are your monitoring goals?
+ What resources will you monitor?
+ How often will you monitor these resources?
+ What monitoring tools will you use?
+ Who will perform the monitoring tasks?
+ Who should be notified when something goes wrong?

After you have defined your monitoring goals and have created your monitoring plan, the next step is to establish a baseline for normal Systems Manager performance in your environment\. You should measure Systems Manager performance at various times and under different load conditions\. As you monitor Systems Manager, you should store a history of monitoring data that you've collected\. You can compare current Systems Manager performance to this historical data to help you to identify normal performance patterns and performance anomalies, and devise methods to address them\.

For example, you can monitor the success or failure of operations such as Automation workflows, the application of patch baselines, maintenance window events, and configuration compliance\.

You can also monitor CPU utilization, disk I/O, and network utilization of your managed instances\. When performance falls outside your established baseline, you might need to reconfigure or optimize the instance to reduce CPU utilization, improve disk I/O, or reduce network traffic\. For more information about monitoring Amazon EC2 instances, see [Monitoring Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring_ec2.html) in the *Amazon EC2 User Guide for Linux Instances*\.

**Topics**
+ [Sending Instance Logs to CloudWatch Logs \(CloudWatch agent\)](monitoring-cloudwatch-agent.md)
+ [Sending Instance Logs to CloudWatch Logs \(SSM Agent\)](monitoring-ssm-agent.md)
+ [Logging AWS Systems Manager API Calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)
+ [Monitoring Systems Manager Events with Amazon CloudWatch Events](monitoring-cloudwatch-events.md)
+ [Configuring Amazon SNS Notifications for AWS Systems Manager](monitoring-sns-notifications.md)