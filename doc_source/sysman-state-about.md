# About State Manager<a name="sysman-state-about"></a>

AWS Systems Manager State Manager is a secure and scalable service that automates the process of keeping your Amazon EC2 and hybrid infrastructure in a state that you define\.

Here's how it works:

**1\. Determine the state you want to apply to your managed instances\.**  
Do you want to ensure that your managed instance are configured with specific applications, such as antivirus or malware applications? Do you want to automate the process of updating the SSM Agent or other AWS packages such as AWSPVDriver? Do you need to ensure that specific ports are closed or open? To get started with State Manager, determine the state that you want to apply to your managed instances\. The state that you want to apply determines which SSM document you use to create a State Manager association\.  
A State Manager *association* is a configuration that is assigned to your managed instances\. The configuration defines the state that you want to maintain on your instances\. For example, an association can specify that antivirus software must be installed and running on your instances, or that certain ports must be closed\. The association specifies a schedule for when the configuration is reapplied\. The association also specifies actions to take when applying the configuration\. For example, an association for antivirus software might run once a day\. If the software is not installed, then State Manager installs it\. If the software is installed, but the service is not running, then the association might instruct State Manager to start the service\.

**2\. Determine if a preconfigured SSM document can help you create the State Manager association\.**  
State Manager uses SSM documents to create an association\. Systems Manager includes dozens of preconfigured SSM documents that you can use to create an association\. Preconfigured documents are ready to perform common tasks like installing applications, configuring Amazon CloudWatch, running Systems Manager Automations, running PowerShell and Shell scripts, and joining a Directory Service domain for Active Directory, to name a few\. You simply need to specify the name of the document and information for the required parameters, and then run the command to create the association\. You can view all SSM documents in the [Systems Manager console](https://console.aws.amazon.com/systems-manager/documents)\.   
You can then choose the name of a document to learn more about each one\. Here are two examples: [AWS\-ConfigureAWSPackage](https://console.aws.amazon.com/systems-manager/documents/AWS-ConfigureAWSPackage/description) and [AWS\-InstallApplication](https://console.aws.amazon.com/systems-manager/documents/AWS-InstallApplication/description)\.

**3\. Create the association\.**  
You can use the AWS Systems Manager console, AWS CLI, AWS Tools for Windows PowerShell, or Systems Manager API to create the association\. When you create the association, you specify the following information:  

1. The parameters for the SSM document \(for example, the path to the application to install or the script to run on the instances\)\.

1. A schedule for when or how often to apply the state\. You can specify a cron or rate expression\. For more information about creating schedules by using cron and rate expressions, see [Cron and Rate Expressions for Associations](reference-cron-and-rate-expressions.md#reference-cron-and-rate-expressions-association)\.

1. Targets for the association\. You can target managed instances by individually specifying IDs, or you can target large groups of managed instances by specifying Amazon EC2 tags\. You can also target all managed instances in the current AWS Region and AWS account\.
When you run the command to create the association, Systems Manager binds the information you specified \(schedule, targets, SSM document, and parameters\) to the managed instances\. The status of the association initially shows `Pending` as the system attempts to reach all targets and immediately apply the state specified in the association\.   
If you create a new association that is scheduled to run while an earlier association is still running, the earlier association is timed out and the new association runs\.
Systems Manager reports the status of the request to create associations on the managed instances\. You can view status details in the console or by using the [DescribeInstanceAssociationsStatus](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeInstanceAssociationsStatus.html) API action\. If you choose to write the output of the command to Amazon S3 when you create an association, you can also view the output in the S3 bucket you specify\.  
For more information, see [Create an Association](sysman-state-assoc.md)\. 

**4\. Monitor and update\.**  
After you create the association, State Manager reapplies the configuration according to the schedule that you defined in the association\. You can view the status of your associations on the [State Manager page](https://console.aws.amazon.com/systems-manager/state-manager) in the console or by directly calling the association ID generated by Systems Manager when you created the association\. For more information, see [Viewing Association Histories](sysman-state-assoc-history.md)\. You can update your association documents and reapply them as necessary\. You can also create multiple versions of an association\. For more information, see [Edit and Create a New Version of an Association](sysman-state-assoc-edit.md)\.