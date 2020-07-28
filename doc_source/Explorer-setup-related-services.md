# Setting up related services<a name="Explorer-setup-related-services"></a>

Explorer and OpsCenter collect information from, or interact with, other AWS services and Systems Manager capabilities\. We recommend that you set up and configure these other services or capabilities before you use Integrated Setup\.

The following table includes tasks that enable Explorer and OpsCenter to collect information from, or interact with, other AWS services and Systems Manager capabilities\. 


****  

| Task | Information | 
| --- | --- | 
|  Verify permissions in Systems Manager Automation  |  Explorer and OpsCenter enable you to remediate issues with AWS resources by using Systems Manager Automation documents \(runbooks\)\. To use this remediation capability, you must have permission to run Systems Manager Automation documents\. For more information, see [Getting started with Automation](automation-setup.md)\.  | 
|  Set up and configure Systems Manager Patch Manager  |  Explorer includes a widget that provides information about patch compliance\. To view this data in Explorer, you must configure patching\. For more information, see [AWS Systems Manager Patch Manager](systems-manager-patch.md)\.  | 
|  Set up and configure Systems Manager State Manager  |  Explorer includes a widget that provides information about State Manager association compliance\. To view this data in Explorer, you must configure State Manager\. For more information, see [AWS Systems Manager State Manager](systems-manager-state.md)\.  | 
|  Enable AWS Config Configuration Recorder  |  Explorer uses data provided by AWS Config configuration recorder to populate widgets with information about your EC2 instances\. To view this data in Explorer, enable AWS Config configuration recorder\. For more information, see [Managing the Configuration Recorder](https://docs.aws.amazon.com/config/latest/developerguide/stop-start-recorder.html)\.  After you enable configuration recorder, Systems Manager can take up to six hours to display data in Explorer widgets that display information about your EC2 instances\.   | 
| Enable AWS Trusted Advisor | Explorer uses data provided by Trusted Advisor to display a status of best practice checks for Amazon EC2 reserved instances in the areas of cost optimization, security, fault tolerance, performance, and service limits\. To view this data in Explorer, you must have a business or enterprise support plan\. For more information, see [AWS Support](https://aws.amazon.com/premiumsupport/)\. | 
| Enable AWS Compute Optimizer | Explorer uses data provided by Compute Optimizer to display details a count of **Under provisioned** and **Over provisioned** EC2 instances, optimization findings, on\-demand pricing details, and recommendations for instance type and price\. To view this data in Explorer, enable Compute Optimizer\. For more information, see [Getting started with AWS Compute Optimizer](https://docs.aws.amazon.com/compute-optimizer/latest/ug/getting-started.html)\. | 