# Monitoring instance performance<a name="fleet-monitoring"></a>

You can use Fleet Manager, a capability of AWS Systems Manager, to view performance data about your instances in real\-time\. The performance data is retrieved from performance counters\.

The following performance counters are available in Fleet Manager:
+ CPU utilization
+ Disk input/output \(I/O\) utilization
+ Network traffic
+ Memory usage

**Note**  
Fleet Manager uses Session Manager, a capability of AWS Systems Manager, to retrieve performance data\. The instance profile attached to your managed instances must provide permissions for Session Manager to use this feature\. For more information about adding Session Manager permissions to an instance profile, see [Adding Session Manager permissions to an existing instance profile](getting-started-add-permissions-to-existing-profile.md)\. Also, AWS Key Management Service \(AWS KMS\) encryption must be turned on in your session preferences to use Fleet Manager features\. For more information about turning on AWS KMS encryption for Session Manager, see [Turn on KMS key encryption of session data \(console\)](session-preferences-enable-encryption.md)\.

**To view performance data with Fleet Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance whose performance you want to monitor\.

1. Choose **View details**\.

1. In the **Tools** menu, choose **Performance counters**\.