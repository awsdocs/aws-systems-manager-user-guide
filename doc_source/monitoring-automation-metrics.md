# Monitoring Automation metrics using Amazon CloudWatch<a name="monitoring-automation-metrics"></a>

*Metrics* are the fundamental concept in Amazon CloudWatch\. A metric represents a time\-ordered set of data points that are published to CloudWatch\. Think of a metric as a variable to monitor and the data points as representing the values of that variable over time\.

Automation is a capability of AWS Systems Manager\. Systems Manager publishes metrics about Automation usage to CloudWatch\. This allows you to set alarms based on those metrics\.

**To view Automation metrics in the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Choose **SSM**\.

1. On the **Metrics** tab, choose **Usage**, and then choose **By AWS Resource**\.

1. In the search box near the list of metrics, enter **SSM**\.

**To view Automation metrics using the AWS CLI**  
Open a command prompt, and use the following command\.

```
aws cloudwatch list-metrics \
    --namespace "AWS/Usage"
```

## Automation metrics<a name="automation-metrics-and-dimensions"></a>

Systems Manager sends the following Automation metrics to CloudWatch\.


| Metric | Description | 
| --- | --- | 
| ConcurrentAutomationUsage  | The number of automations running at the same time in the current AWS account and AWS Region\. | 
| QueuedAutomationUsage  | The number of automations currently queued that have not started and have a status of Pending\. | 

For more information about working with CloudWatch metrics, see the following topics in the *Amazon CloudWatch User Guide*:
+ [Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)
+ [Using Amazon CloudWatch metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)
+ [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)