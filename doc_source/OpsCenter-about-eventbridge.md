# EventBridge<a name="OpsCenter-about-eventbridge"></a>

Amazon EventBridge delivers a stream of events that describe changes in AWS resources\. When you enable OpsCenter using Integrated Setup, it integrates EventBridge with OpsCenter, and enables default EventBridge rules\. Based on these rules, EventBridge creates OpsItems\. Using rules, you can filter and route events to OpsCenter for investigation and remediation\. 

**Note**  
Amazon EventBridge \(formerly Amazon CloudWatch Events\) provides all functionality of CloudWatch Events and some new features, such as custom event buses, third\-party event sources and schema registry\.

Following are some rules that you can configure in EventBridge to create an OpsItem: 
+ Security Hub: security alert issued
+ Amazon DynamoDB a throttling event
+ Amazon Elastic Compute Cloud Auto Scaling: failure to launch an instance 
+ Systems Manager: failure to run an automation 
+ AWS Health: an alert for scheduled maintenance
+ Amazon EC2: instance state changed from running to stop 

Based on your requirements, you can either create a rule or edit an existing rule to create an OpsItems\. For instructions on how to edit a rule to create an OpsItem, see [Configure EventBridge rules to create OpsItems](OpsCenter-automatically-create-OpsItems-2.md)\.