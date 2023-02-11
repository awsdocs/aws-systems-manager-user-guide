# Configuring an existing CloudWatch alarm to create OpsItems \(programmatically\)<a name="OpsCenter-configuring-an-existing-alarm-programmatically"></a>

You can configure Amazon CloudWatch alarms to create OpsItems programmatically by using the AWS Command Line Interface \(AWS CLI\), AWS CloudFormation templates, or Java code snippets\.

**Topics**
+ [Before you begin](#OpsCenter-create-OpsItems-from-CloudWatch-Alarms-before-you-begin)
+ [Configuring CloudWatch alarms to create OpsItems \(AWS CLI\)](#OpsCenter-create-OpsItems-from-CloudWatch-Alarms-manually-configure-cli)
+ [Configuring CloudWatch alarms to create or update OpsItems \(CloudFormation\)](#OpsCenter-create-OpsItems-from-CloudWatch-Alarms-programmatically-configure-CloudFormation)
+ [Configuring CloudWatch alarms to create or update OpsItems \(Java\)](#OpsCenter-create-OpsItems-from-CloudWatch-Alarms-programmatically-configure-java)

## Before you begin<a name="OpsCenter-create-OpsItems-from-CloudWatch-Alarms-before-you-begin"></a>

If you edit an existing alarm programmatically or create an alarm that creates OpsItems, you must specify an Amazon Resource Name \(ARN\)\. This ARN identifies Systems Manager OpsCenter as the target for OpsItems created from the alarm\. You can customize the ARN so that OpsItems created from the alarm include specific information such as severity or category\. Each ARN includes the information described in the following table\.


****  

| Parameter | Details | 
| --- | --- | 
|  `Region` \(required\)  |  The AWS Region where the alarm exists\. For example: `us-west-2`\. For information about AWS Regions where you can use OpsCenter, see [AWS Systems Manager endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/ssm.html)\.  | 
|  `account_ID` \(required\)  |  The same AWS account ID used to create the alarm\. For example: `123456789012`\. The account ID must be followed by a colon \(`:`\) and the parameter `opsitem` as shown in the following examples\.  | 
|  `severity` \(required\)  |  A user\-defined severity level for OpsItems created from the alarm\. Valid values: `1`, `2`, `3`, `4`  | 
|  `Category`\(optional\)  |  A category for OpsItems created from the alarm\. Valid values: `Availability`, `Cost`, `Performance`, `Recovery`, and `Security`\.  | 

Create the ARN by using the following syntax\. This ARN doesn't include the optional `Category` parameter\.

```
arn:aws:ssm:Region:account_ID:opsitem:severity
```

Following is an example\.

```
arn:aws:ssm:us-west-2:123456789012:opsitem:3
```

To create an ARN that uses the optional `Category` parameter, use the following syntax\.

```
arn:aws:ssm:Region:account_ID:opsitem:severity#CATEGORY=category_name
```

Following is an example\.

```
arn:aws:ssm:us-west-2:123456789012:opsitem:3#CATEGORY=Security
```

## Configuring CloudWatch alarms to create OpsItems \(AWS CLI\)<a name="OpsCenter-create-OpsItems-from-CloudWatch-Alarms-manually-configure-cli"></a>

This command requires that you specify an ARN for the `alarm-actions` parameter\. For information about how to create the ARN, see [Before you begin](#OpsCenter-create-OpsItems-from-CloudWatch-Alarms-before-you-begin)\.

**To configure a CloudWatch alarm to create OpsItems \(AWS CLI\)**

1. Install and configure the AWS Command Line Interface \(AWS CLI\), if you haven't already\.

   For information, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)\.

1. Run the following command to collect information about the alarm that you want to configure\.

   ```
   aws cloudwatch describe-alarms --alarm-names "alarm name"
   ```

1. Run the following command to update an alarm\. Replace each *example resource placeholder* with your own information\.

   ```
   aws cloudwatch put-metric-alarm --alarm-name name \
   --alarm-description "description" \
   --metric-name name --namespace namespace \
   --statistic statistic --period value --threshold value \
   --comparison-operator value \
   --dimensions "dimensions" --evaluation-periods value \
       --alarm-actions arn:aws:ssm:Region:account_ID:opsitem:severity#CATEGORY=category_name \
   --unit unit
   ```

   Here's an example\.

------
#### [ Linux & macOS ]

   ```
   aws cloudwatch put-metric-alarm --alarm-name cpu-mon \
   --alarm-description "Alarm when CPU exceeds 70 percent" \
   --metric-name CPUUtilization --namespace AWS/EC2 \
   --statistic Average --period 300 --threshold 70 \
   --comparison-operator GreaterThanThreshold \
   --dimensions "Name=InstanceId,Value=i-12345678" --evaluation-periods 2 \
   --alarm-actions arn:aws:ssm:us-east-1:123456789012:opsitem:3#CATEGORY=Security \
   --unit Percent
   ```

------
#### [ Windows ]

   ```
   aws cloudwatch put-metric-alarm --alarm-name cpu-mon ^
   --alarm-description "Alarm when CPU exceeds 70 percent" ^
   --metric-name CPUUtilization --namespace AWS/EC2 ^
   --statistic Average --period 300 --threshold 70 ^
   --comparison-operator GreaterThanThreshold ^
   --dimensions "Name=InstanceId,Value=i-12345678" --evaluation-periods 2 ^
   --alarm-actions arn:aws:ssm:us-east-1:123456789012:opsitem:3#CATEGORY=Security ^
   --unit Percent
   ```

------

## Configuring CloudWatch alarms to create or update OpsItems \(CloudFormation\)<a name="OpsCenter-create-OpsItems-from-CloudWatch-Alarms-programmatically-configure-CloudFormation"></a>

This section includes AWS CloudFormation templates that you can use to configure CloudWatch alarms to automatically create or update OpsItems\. Each template requires that you specify an ARN for the `AlarmActions` parameter\. For information about how to create the ARN, see [Before you begin](#OpsCenter-create-OpsItems-from-CloudWatch-Alarms-before-you-begin)\.

**Metric alarm** – Use the following CloudFormation template to create or update a CloudWatch metric alarm\. The alarm specified in this template monitors Amazon Elastic Compute Cloud \(Amazon EC2\) instance status checks\. If the alarm enters the `ALARM` state, it creates an OpsItem in OpsCenter\. 

```
    {
      "AWSTemplateFormatVersion": "2010-09-09",
      "Parameters" : {
        "RecoveryInstance" : {
          "Description" : "The EC2 instance ID to associate this alarm with.",
          "Type" : "AWS::EC2::Instance::Id"
        }
      },
      "Resources": {
        "RecoveryTestAlarm": {
          "Type": "AWS::CloudWatch::Alarm",
          "Properties": {
            "AlarmDescription": "Run a recovery action when instance status check fails for 15 consecutive minutes.",
            "Namespace": "AWS/EC2" ,
            "MetricName": "StatusCheckFailed_System",
            "Statistic": "Minimum",
            "Period": "60",
            "EvaluationPeriods": "15",
            "ComparisonOperator": "GreaterThanThreshold",
            "Threshold": "0",
            "AlarmActions": [ {"Fn::Join" : ["", ["arn:arn:aws:ssm:Region:account_ID:opsitem:severity#CATEGORY=category_name", { "Ref" : "AWS::Partition" }, ":ssm:", { "Ref" : "AWS::Region" }, { "Ref" : "AWS:: AccountId" }, ":opsitem:3" ]]} ],
            "Dimensions": [{"Name": "InstanceId","Value": {"Ref": "RecoveryInstance"}}]
          }
        }
      }
    }
```

**Composite alarm** – Use the following CloudFormation template to create or update a composite alarm\. A composite alarm consists of multiple metric alarms\. If the alarm enters the `ALARM` state, it creates an OpsItem in OpsCenter\.

```
"Resources":{
       "HighResourceUsage":{
          "Type":"AWS::CloudWatch::CompositeAlarm",
          "Properties":{
             "AlarmName":"HighResourceUsage",
             "AlarmRule":"(ALARM(HighCPUUsage) OR ALARM(HighMemoryUsage)) AND NOT ALARM(DeploymentInProgress)",
             "AlarmActions":"arn:aws:ssm:Region:account_ID:opsitem:severity#CATEGORY=category_name",
             "AlarmDescription":"Indicates that the system resource usage is high while no known deployment is in progress"
          },
          "DependsOn":[
             "DeploymentInProgress",
             "HighCPUUsage",
             "HighMemoryUsage"
          ]
       },
       "DeploymentInProgress":{
          "Type":"AWS::CloudWatch::CompositeAlarm",
          "Properties":{
             "AlarmName":"DeploymentInProgress",
             "AlarmRule":"FALSE",
             "AlarmDescription":"Manually updated to TRUE/FALSE to disable other alarms"
          }
       },
       "HighCPUUsage":{
          "Type":"AWS::CloudWatch::Alarm",
          "Properties":{
             "AlarmDescription":"CPUusageishigh",
             "AlarmName":"HighCPUUsage",
             "ComparisonOperator":"GreaterThanThreshold",
             "EvaluationPeriods":1,
             "MetricName":"CPUUsage",
             "Namespace":"CustomNamespace",
             "Period":60,
             "Statistic":"Average",
             "Threshold":70,
             "TreatMissingData":"notBreaching"
          }
       },
       "HighMemoryUsage":{
          "Type":"AWS::CloudWatch::Alarm",
          "Properties":{
             "AlarmDescription":"Memoryusageishigh",
             "AlarmName":"HighMemoryUsage",
             "ComparisonOperator":"GreaterThanThreshold",
             "EvaluationPeriods":1,
             "MetricName":"MemoryUsage",
             "Namespace":"CustomNamespace",
             "Period":60,
             "Statistic":"Average",
             "Threshold":65,
             "TreatMissingData":"breaching"
          }
       }
    }
```

## Configuring CloudWatch alarms to create or update OpsItems \(Java\)<a name="OpsCenter-create-OpsItems-from-CloudWatch-Alarms-programmatically-configure-java"></a>

This section includes Java code snippets that you can use to configure CloudWatch alarms to automatically create or update OpsItems\. Each snippet requires that you specify an ARN for the `validSsmActionStr` parameter\. For information about how to create the ARN, see [Before you begin](#OpsCenter-create-OpsItems-from-CloudWatch-Alarms-before-you-begin)\.

**A specific alarm** – Use the following Java code snippet to create or update a CloudWatch alarm\. The alarm specified in this template monitors Amazon EC2 instance status checks\. If the alarm enters the `ALARM` state, it creates an OpsItem in OpsCenter\.

```
import com.amazonaws.services.cloudwatch.AmazonCloudWatch;
    import com.amazonaws.services.cloudwatch.AmazonCloudWatchClientBuilder;
    import com.amazonaws.services.cloudwatch.model.ComparisonOperator;
    import com.amazonaws.services.cloudwatch.model.Dimension;
    import com.amazonaws.services.cloudwatch.model.PutMetricAlarmRequest;
    import com.amazonaws.services.cloudwatch.model.PutMetricAlarmResult;
    import com.amazonaws.services.cloudwatch.model.StandardUnit;
    import com.amazonaws.services.cloudwatch.model.Statistic;
     
    private void putMetricAlarmWithSsmAction() {
        final AmazonCloudWatch cw =
                AmazonCloudWatchClientBuilder.defaultClient();
     
        Dimension dimension = new Dimension()
                .withName("InstanceId")
                .withValue(instanceId);
     
        String validSsmActionStr = "arn:aws:ssm:Region:account_ID:opsitem:severity#CATEGORY=category_name";
     
        PutMetricAlarmRequest request = new PutMetricAlarmRequest()
                .withAlarmName(alarmName)
                .withComparisonOperator(
                        ComparisonOperator.GreaterThanThreshold)
                .withEvaluationPeriods(1)
                .withMetricName("CPUUtilization")
                .withNamespace("AWS/EC2")
                .withPeriod(60)
                .withStatistic(Statistic.Average)
                .withThreshold(70.0)
                .withActionsEnabled(false)
                .withAlarmDescription(
                        "Alarm when server CPU utilization exceeds 70%")
                .withUnit(StandardUnit.Seconds)
                .withDimensions(dimension)
                .withAlarmActions(validSsmActionStr);
     
        PutMetricAlarmResult response = cw.putMetricAlarm(request);
    }
```

**Update all alarms** – Use the following Java code snippet to update all CloudWatch alarms in your AWS account to create OpsItems when an alarm enters the `ALARM` state\. 

```
import com.amazonaws.services.cloudwatch.AmazonCloudWatch;
    import com.amazonaws.services.cloudwatch.AmazonCloudWatchClientBuilder;
    import com.amazonaws.services.cloudwatch.model.DescribeAlarmsRequest;
    import com.amazonaws.services.cloudwatch.model.DescribeAlarmsResult;
    import com.amazonaws.services.cloudwatch.model.MetricAlarm;
     
    private void listMetricAlarmsAndAddSsmAction() {
        final AmazonCloudWatch cw = AmazonCloudWatchClientBuilder.defaultClient();
     
        boolean done = false;
        DescribeAlarmsRequest request = new DescribeAlarmsRequest();
     
        String validSsmActionStr = "arn:aws:ssm:Region:account_ID:opsitem:severity#CATEGORY=category_name";
     
        while(!done) {
     
            DescribeAlarmsResult response = cw.describeAlarms(request);
     
            for(MetricAlarm alarm : response.getMetricAlarms()) {
                // assuming there are no alarm actions added for the metric alarm
                alarm.setAlarmActions(ImmutableList.of(validSsmActionStr));
            }
     
            request.setNextToken(response.getNextToken());
     
            if(response.getNextToken() == null) {
                done = true;
            }
        }
    }
```