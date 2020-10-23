# Monitoring Run Command metrics using Amazon CloudWatch<a name="monitoring-cloudwatch-metrics"></a>

*Metrics* are the fundamental concept in CloudWatch\. A metric represents a time\-ordered set of data points that are published to CloudWatch\. Think of a metric as a variable to monitor, and the data points as representing the values of that variable over time\.

AWS Systems Manager publishes metrics about the status of Run Command commands to CloudWatch, enabling you to set alarms based on those metrics\. These statistics are recorded for an extended period so you can access historical information and gain a better perspective on the success rate of commands run in your AWS account\. 

The terminal status values for commands for which you can track metrics include `Success`, `Failed`, and `Delivery Timed Out`\. For example, for an SSM command document set to run every hour, you can configure an alarm to notify you when a status of `Success` is not reported for any of those hours\. For more information about command status values, see [Understanding command statuses](monitor-commands.md)\.

**To view metrics in the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Select the **SSM Run Command** tile\.

**To view metrics using the AWS CLI**  
Open a command prompt, and use the following command:

```
aws cloudwatch list-metrics --namespace "AWS/SSM-RunCommand"
```

To list all available metrics, use the following command:

```
aws cloudwatch list-metrics --namespace "AWS/SSM-RunCommand"
```

## Systems Manager Run Command metrics and dimensions<a name="metrics-and-dimensions"></a>

Systems Manager sends Systems Manager Run Command command metrics to CloudWatch one time every minute\.

Systems Manager sends the following command metrics to CloudWatch\.

**Note**  
All of these metrics use `Count` as the unit, so `Sum` and `SampleCount` are the most useful statistics\.


| Metric | Description | 
| --- | --- | 
| CommandsDeliveryTimedOut  | The number of commands that have a terminal status of Delivery Timed Out\.  | 
| CommandsFailed  | The number of commands that have a terminal status of Failed\. | 
| CommandsSucceeded  | The number of commands that have a terminal status of Success\. | 

For more information about working with CloudWatch metrics, see the following topics in the *Amazon CloudWatch User Guide*:
+ [Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)
+ [Using Amazon CloudWatch metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)
+ [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)