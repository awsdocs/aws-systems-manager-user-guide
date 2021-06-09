# Viewing monitoring information<a name="application-manager-working-viewing-monitors"></a>

In Application Manager, a component of AWS Systems Manager, the **Monitoring** tab displays Amazon CloudWatch alarm status for resources in an application\.

**Actions you can perform on this page**  
You can perform the following actions on this page:
+ Choose a service name in the **Alarms by AWS service** section to open CloudWatch to the selected service and alarm\.
+ Adjust the time period for data displayed in widgets in the **Recent alarms** section by selecting one of the predefined time period values\. You can choose **custom** to define your own time period\.  
![\[The time period controls on the Application Manager Monitoring tab.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/application-manager-Monitoring-1.png)
+ Hover your cursor over a widget in the **Recent alarms** section to view a data pop\-up for a specific time\.  
![\[An alarm widget in the Recent alarms section of the Application Manager Monitoring tab.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/application-manager-Monitoring-2.png)
+ Choose the options menu in a widget to view display options\. Choose **Enlarge** to expand a widget\. Choose **Refresh** to update the data in a widget\. Click and drag your cursor in a widget data display to select a specific range\. You can then choose **Apply time range**\.  
![\[Context-sensitive alarm widget options in the Recent alarms section of the Application Manager Monitoring tab.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/application-manager-Monitoring-3.png)
+ Choose the **Actions** menu to view alarm data **Override** options, which include the following:
  + Choose whether your widget displays live data\. Live data is data published within the last minute that has not been fully aggregated\. If live data is turned off, only data points with an aggregation period of at least one minute in the past are shown\. For example, when using 5\-minute periods, the data point for 12:35 would be aggregated from 12:35 to 12:40, and displayed at 12:41\.

    If live data is turned on, the most recent data point is shown as soon as any data is published in the corresponding aggregation interval\. Each time you refresh the display, the most recent data point might change as new data within that aggregation period is published\.
  + Specify a time period for live data\.
  + Link the charts in the **Recent alarms** section, so that when you zoom in or zoom out on one chart, the other chart zooms in or zooms out at the same time\. You can unlink charts to limit zoom to one chart\. 
  + Hide Auto Scaling alarms\.  
![\[The Action menu of the Application Manager Monitoring tab.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/application-manager-Monitoring-4.png)

**To open the **Monitoring** tab**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1. In the **Applications** section, choose a category\. If you want to open an application you created manually in Application Manager, choose **Custom applications**\.

1. Choose the application in the list\. Application Manager opens the **Overview** tab\.

1. Choose the **Monitoring** tab\.