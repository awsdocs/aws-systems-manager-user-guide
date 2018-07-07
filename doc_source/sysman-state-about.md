# About State Manager<a name="sysman-state-about"></a>

Systems Manager State Manager is a secure and scalable configuration management service that ensures your Amazon EC2 and hybrid infrastructure is in an intended or consistent state, which you define\. 

State Manager works as follows:

1. You determine the state you want to apply to your managed instances\. 

   For example, determine the applications to bootstrap or the network settings to configure\. You can specify the details of the state as parameters at runtime by using an AWS preconfigured document\. Or, you can create your own document and either specify the state directly in the document or as parameters at runtime\. These documents, written in JSON or YAML, are called *SSM documents*\. 

   An SSM document can include multiple actions or steps \(for example, multiple commands to run\)\. The two types of SSM documents that State Manager uses are *Command* documents and *Policy* documents\. For more information about SSM documents, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

1. You specify a schedule for when or how often to apply the state\. You can specify a cron or rate expression\.

1. You specify the targets for the state\. 

   You can target managed instances \(Amazon EC2 instances, or machines in your hybrid environment that are configured for Systems Manager\.\) You can target instances by specifying one or more instance IDs, or you target large groups of managed instances by targeting EC2 tags\. Using the AWS CLI, AWS Tools for Windows PowerShell, or the Systems Manager SDK, you can identify targets by specifying multiple tags\.

1. You bind this information \(schedule, targets, documents, parameters\) to the managed instances\. 

   The binding of this information to the targets is called called * creating an association*\. You can create an association by using the Amazon EC2 console, the AWS CLI, AWS Tools for Windows PowerShell, or AWS SDKs\.

1. After you send the request to create an association, the status of the association shows "Pending"\. The system attempts to reach all targets and immediately apply the state specified in the association\. 
**Note**  
If you create a new association that is scheduled to run while an earlier association is still running, the earlier association is timed out and the new association runs\.

1. Systems Manager reports the status of the request for each instance targeted by the request\. 

   You can view status details in the EC2 console or by using the [DescribeInstanceAssociationsStatus](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeInstanceAssociationsStatus.html) API action\. If you choose to write the output of the command to S3 when you create an association, you can also view the output in the Amazon S3 bucket you specify\.

1. After you create the association, State Manager reapplies the state according to the schedule defined in the association\. 

   You can update your association documents and reapply them as necessary\. You can also create multiple versions of an association\.