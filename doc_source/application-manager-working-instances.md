# Working with your application instances<a name="application-manager-working-instances"></a>

Application Manager integrates with Amazon Elastic Compute Cloud \(Amazon EC2\) to display information about your instances in the context of an application\. Application Manager displays instance state, status, and Amazon EC2 Auto Scaling health for a selected application in a graphical format\. The **Instances** tab also includes a table with the following information for each instance in your application:
+ Instance state \(Pending, Stopping, Running, Stopped\)
+ Ping status for SSM Agent
+ Status and name of the most recent Systems Manager Automation runbook processed on the instance
+ A count of Amazon CloudWatch Logs alarms per state\.
  + `ALARM` – The metric or expression is outside of the defined threshold\.
  + `OK` – The metric or expression is within the defined threshold\.
  + `INSUFFICIENT_DATA` – The alarm has just started, the metric is not available, or not enough data is available for the metric to determine the alarm state\.
+ Auto Scaling group health for the parent and individual autoscaling groups

If you choose an instance in the **All instances** table, Application Manager displays information about that instance on four tabs:
+ **Details** – All of the instance details from Amazon EC2, including the Amazon Machine Image \(AMI\), DNS information, IP address information, and more\.
+ **Health** – The current status as provided by EC2 system and instance status checks\.
+ **Execution history** – Execution logs for Systems Manager Automation runbooks and API calls processed by the instance\.
+ **CloudWatch alarms** – The name, state, and more for any CloudWatch alarms raised by the instance\.

**Actions you can perform on this page**  
You can perform the following actions on this page:
+ Start, stop, and terminate instances\.
+ Apply a Chef recipe\.
+ Attach instances to, or detach instances from, an Auto Scaling group\.

**To open the **Instances** tab**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1. In the **Applications** section, choose a category\. If you want to open an application that you created manually in Application Manager, choose **Custom applications**\.

1. Choose the application in the list\. Application Manager opens the **Overview** tab\.

1. Choose the **Instances** tab\.