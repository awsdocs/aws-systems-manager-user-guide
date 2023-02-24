# About State Manager<a name="sysman-state-about"></a>

State Manager, a capability of AWS Systems Manager, is a secure and scalable service that automates the process of keeping your Amazon Elastic Compute Cloud \(Amazon EC2\) instances, edge devices, and hybrid infrastructure in a state that you define\.

Here's how State Manager works:

**1\. Determine the state you want to apply to your AWS resources\.**  
Do you want to guarantee that your managed node are configured with specific applications, such as antivirus or malware applications? Do you want to automate the process of updating the SSM Agent or other AWS packages such as `AWSPVDriver`? Do you need to guarantee that specific ports are closed or open? To get started with State Manager, determine the state that you want to apply to your AWS resources\. The state that you want to apply determines which SSM document you use to create a State Manager association\.  
A State Manager *association* is a configuration that you assign to your AWS resources\. The configuration defines the state that you want to maintain on your resources\. For example, an association can specify that antivirus software must be installed and running on a managed node, or that certain ports must be closed\.  
An association specifies a schedule for when to apply the configuration and the targets for the association\. For example, an association for antivirus software might run once a day on all managed nodes in an AWS account\. If the software isn't installed on a node, then the association could instruct State Manager to install it\. If the software is installed, but the service isn't running, then the association could instruct State Manager to start the service\.

**2\. Determine if a preconfigured SSM document can help you create the desired state on your AWS resources\.**  
Systems Manager includes dozens of preconfigured SSM documents that you can use to create an association\. Preconfigured documents are ready to perform common tasks like installing applications, configuring Amazon CloudWatch, running AWS Systems Manager automations, running PowerShell and Shell scripts, and joining managed nodes to a directory service domain for Active Directory\.  
You can view all SSM documents in the [Systems Manager console](https://console.aws.amazon.com/systems-manager/documents)\. Choose the name of a document to learn more about each one\. Here are two examples: [https://console.aws.amazon.com/systems-manager/documents/AWS-ConfigureAWSPackage/description](https://console.aws.amazon.com/systems-manager/documents/AWS-ConfigureAWSPackage/description) and [https://console.aws.amazon.com/systems-manager/documents/AWS-InstallApplication/description](https://console.aws.amazon.com/systems-manager/documents/AWS-InstallApplication/description)\.

**3\. Create an association\.**  
You can create an association by using the Systems Manager console, the AWS Command Line Interface \(AWS CLI\), AWS Tools for Windows PowerShell \(Tools for Windows PowerShell\), or the Systems Manager API\. When you create an association, you specify the following information:  
+ A name for the association\.
+ The parameters for the SSM document \(for example, the path to the application to install or the script to run on the nodes\)\.
+ Targets for the association\. You can target managed nodes by specifying tags, by choosing individual node IDs, or by choosing a group in AWS Resource Groups\. You can also target *all* managed nodes in the current AWS Region and AWS account\.
+ A schedule for when or how often to apply the state\. You can specify a cron or rate expression\. For more information about creating schedules by using cron and rate expressions, see [Cron and rate expressions for associations](reference-cron-and-rate-expressions.md#reference-cron-and-rate-expressions-association)\.
**Note**  
State Manager doesn't currently support specifying months in cron expressions for associations\.
When you run the command to create the association, Systems Manager binds the information you specified \(schedule, targets, SSM document, and parameters\) to the targeted resources\. The status of the association initially shows "Pending" as the system attempts to reach all targets and *immediately* apply the state specified in the association\.   
If you create a new association that is scheduled to run while an earlier association is still running, the earlier association times out and the new association runs\.
Systems Manager reports the status of the request to create associations on the resources\. You can view status details in the console or \(for managed nodes\) by using the [DescribeInstanceAssociationsStatus](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeInstanceAssociationsStatus.html) API operation\. If you choose to write the output of the command to Amazon Simple Storage Service \(Amazon S3\) when you create an association, you can also view the output in the Amazon S3 bucket you specified\.  
For more information, see [Creating associations](sysman-state-assoc.md)\.   
API operations that are initiated by the SSM document during an association run are not logged in AWS CloudTrail\.

**4\. Monitor and update\.**  
After you create the association, State Manager reapplies the configuration according to the schedule that you defined in the association\. You can view the status of your associations on the [State Manager page](https://console.aws.amazon.com/systems-manager/state-manager) in the console or by directly calling the association ID generated by Systems Manager when you created the association\. For more information, see [Viewing association histories](sysman-state-assoc-history.md)\. You can update your association documents and reapply them as necessary\. You can also create multiple versions of an association\. For more information, see [Editing and creating a new version of an association](sysman-state-assoc-edit.md)\.

## When are associations applied to resources?<a name="state-manager-about-scheduling"></a>

When you create an association, you specify an SSM document that defines the configuration, a list of target resources, and a schedule for applying the configuration\. By default, State Manager runs the association when you create it and then according to your schedule\. State Manager also attempts to run the association in the following situations: 
+ **Association edit** – State Manager runs the association after a user edits and saves their changes to any of the following association fields: `DOCUMENT_VERSION`, `PARAMETERS`, `SCHEDULE_EXPRESSION`, `OUTPUT_S3_LOCATION`\.
+ **Document edit** – State Manager runs the association after a user edits and saves changes to the SSM document that defines the association's configuration state\. Specifically, the association runs after the following edits to the document:
  + A user specifies a new $DEFAULT document version and the association was created using the $DEFAULT version\. 
  + A user updates a document and the association was created using the $LATEST version\.
  + A user deletes the document that was specified when the association was created\.
+ **Parameter Store parameter value change** – State Manager runs the association after a user edits the value of a parameter defined in the association\.
+ **Manual start** – State Manager runs the association when initiated by the user from either the Systems Manager console or programmatically\.
+ **Target changes** – State Manager runs the association after any of the following activity occurs on a target instance:
  + An instance comes online for the first time\.
  + An instance comes online after missing a scheduled association run\.
  + An instance comes online after being stopped for more than 30 days\.
**Note**  
Target updates don't affect associations created using Systems Manager Automation\.