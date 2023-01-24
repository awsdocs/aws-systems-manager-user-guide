# Resource Scheduler<a name="quick-setup-scheduler"></a>

With Quick Setup, a capability of AWS Systems Manager, you can configure Resource Scheduler to automate the starting and stopping of Amazon Elastic Compute Cloud \(Amazon EC2\) instances\.

This Quick Setup configuration helps you reduce operational costs by starting and stopping instances according to the schedule that you specify\. This capability helps you avoid incurring unnecessary costs for running instances when they’re not needed\. For example, you currently might leave your instances running constantly, even though they’re only used 10 hours a day, 5 days a week\. Instead, you can schedule your instances to stop every day after business hours\. As a result, there would be 70 percent savings for those instances because the running time is reduced from 168 hours to 50 hours\.

With Resource Scheduler, you can choose to automatically stop and start instances across multiple AWS Regions and AWS accounts according to a schedule you define\. The Quick Setup configuration targets Amazon EC2 instances using the tag key and value that you specify\. Only the instances with a tag matching the value that you specify in your configuration are stopped or started by Resource Scheduler\.

An individual configuration supports scheduling up to 5,000 instances per Region\. If your case requires more than 5,000 instances to be scheduled in a given Region, you must create multiple configurations\. Tag your instances accordingly so each configuration is managing up to 5,000 instances\. When creating multiple Resource Scheduler Quick Setup configurations, you must specify different tag key values\. For example, one configuration can use the tag key “Env” with the value “Prod”, while another uses “Env” and “Dev”\.

If you delete your configuration, instances are no longer stopped and started according to the previously defined schedule\. In rare cases, instances might not successfully stop or start due to API operation failures\.

Resource Scheduler starts the tagged instances only if they are in the `stopped` state\. Similarly, instances are only stopped if they are in the `running` state\. Resource Scheduler operates on an event driven model and only starts or stops instances at the times that you specify\. For example, you create a schedule that starts instances at 9 AM\. Resource Scheduler starts all instances associated with the tag you specify that are in the `stopped` state at 9 AM\. If the instances are manually stopped at a later time, Resource Scheduler will not start them again to maintain the `running` state\. Similarly, if an instance is started manually after it was stopped according to your schedule, Resource Scheduler will not stop the instance again\.

If you create a schedule with a start time that is later than the stop time, Resource Scheduler assumes your instances run overnight\. For example, you create a schedule that starts instances at 9 PM, and stops instances at 7 AM\. Resource Scheduler starts all instances associated with the tag you specify that are in the `stopped` state at 9 PM, and stops them at 7 AM the following day\. For overnight schedules, the start time applies to the days you select for your schedule\. However, the stop time applies to the following day in your schedule\.

To set up scheduling for Amazon EC2 instances, perform the following tasks in the AWS Systems Manager Quick Setup console\.

**To set up instance scheduling with Quick Setup**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Quick Setup**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Quick Setup** in the navigation pane\.

1. Choose **Create**\.

1. Choose **Resource Scheduler**, and then choose **Next**\.

1. In the **Instance tag** section, specify the tag key and value applied to the instances you want to associate with your schedule\.

1. In the **Schedule options** section, specify the time zone, days, and times you want to start and stop your instances\.

1. In the **Targets** section, choose whether to set scheduling for a **Custom** group of organizational units \(OUs\), or the **Current account** you're signed in to:
   + **Custom** – In the **Target OUs** section, select the OUs where you want to set up scheduling\. Next, in the **Target Regions** section, select the Regions where you want to set up scheduling\.
   + **Current account** – Select **Current Region** or **Choose Regions**\. If you selected **Choose Regions**, choose the **Target Regions** where you want to set up scheduling\.

1. Verify the schedule information in the **Summary** section\.

1. Choose **Create**\.