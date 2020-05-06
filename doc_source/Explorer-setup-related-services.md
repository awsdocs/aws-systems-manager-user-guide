# Setting up related services<a name="Explorer-setup-related-services"></a>

Explorer and OpsCenter collect information from, or interact with, other AWS services and Systems Manager capabilities\. We recommend that you set up and configure these other services or capabilities before you use Integrated Setup\.

The following table includes tasks that enable Explorer and OpsCenter to collect information from, or interact with, other AWS services and Systems Manager capabilities\. 


****  

| Task | Information | 
| --- | --- | 
|  Verify permissions in Systems Manager Automation  |  Explorer and OpsCenter enable you to remediate issues with AWS resources by using Systems Manager Automation documents \(runbooks\)\. To use this remediation capability, you must have permission to run Systems Manager Automation documents\. For more information, see [Getting started with Automation](automation-setup.md)\.  | 
|  Set up and configure Systems Manager Patch Manager  |  Explorer includes a widget that provides information about patch compliance\. To view data in this widget, you must configure patching\. For more information, see [AWS Systems Manager Patch Manager](systems-manager-patch.md)\.  | 
|  Enable AWS Config Configuration Recorder  |  Explorer uses data provided by AWS Config configuration recorder to populate widgets with information about your EC2 instances\. To ensure that these widgets display data, enable AWS Config configuration recorder\. For more information, see [Managing the Configuration Recorder](https://docs.aws.amazon.com/config/latest/developerguide/stop-start-recorder.html)\.  After you enable configuration recorder, Systems Manager can take up to six hours to display data in Explorer widgets that display information about your EC2 instances\.   | 
| Enable AWS Trusted Advisor | Explorer uses data provided by Trusted Advisor to display a status of best practice checks and recommendations for cost optimization, security, fault tolerance, performance, and service limits\. To display this information in Explorer, you must have a business or enterprise support plan\. For more information, see [AWS Support](https://aws.amazon.com/premiumsupport/)\.  | 