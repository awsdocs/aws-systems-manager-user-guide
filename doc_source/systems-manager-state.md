# AWS Systems Manager State Manager<a name="systems-manager-state"></a>

AWS Systems Manager State Manager is a secure and scalable configuration management service that automates the process of keeping your Amazon EC2 and hybrid infrastructure in a state that you define\.

Some of the tasks you can automate using State Manager, to run on schedules you specify, include:
+ Bootstrap instances with specific software at start\-up
+ Download and update agents on a defined schedule, including SSM Agent
+ Configure network settings
+ Join instances to a Windows domain \(Windows instances only\)
+ Patch instances with software updates throughout their lifecycle
+ Run scripts on Linux and Windows managed instances throughout their lifecycle

State Manager integrates with AWS CloudTrail to provide a record of all executions that you can audit, and Amazon CloudWatch Events to track state changes\. You can also choose to store and view detailed command output in Amazon S3\.

**Getting Started with State Manager**

To get started with State Manager, complete the tasks described in the following table\.


****  

| Task | For More Information | 
| --- | --- | 
|  Update SSM Agent on your managed instances to the latest version  |  [Installing and Configuring SSM Agent](ssm-agent.md)  | 
|  Verify Systems Manager prerequisites  |  [Systems Manager Prerequisites](systems-manager-prereqs.md)  | 
|  Choose a predefined AWS Command or Policy type document and specify parameters at runtime\. \-or\- Create a document that defines the actions to perform on your instances\.  |  [Creating Systems Manager Documents](create-ssm-doc.md)  | 
|  Create and apply the association to your instances  |  [Create an Association \(Console\)](sysman-state-assoc.md)  | 

**Related Content**  
See the following blog posts for additional examples of how to use State Manager:
+ [Combating Configuration Drift Using Amazon EC2 Systems Manager and Windows PowerShell DSC](https://aws.amazon.com/blogs/mt/combating-configuration-drift-using-amazon-ec2-systems-manager-and-windows-powershell-dsc/)
+ [Running Ansible Playbooks using Amazon EC2 Systems Manager Run Command and State Manager](https://aws.amazon.com/blogs/mt/running-ansible-playbooks-using-ec2-systems-manager-run-command-and-state-manager/)
+ [Configure Amazon EC2 Instances in an Auto Scaling Group Using State Manager](https://aws.amazon.com/blogs/mt/configure-amazon-ec2-instances-in-an-auto-scaling-group-using-state-manager/)

**Topics**
+ [About State Manager](sysman-state-about.md)
+ [Sample State Manager Documents](sysman-state-sampledocs.md)
+ [Working with Associations in Systems Manager](systems-manager-associations.md)
+ [Systems Manager State Manager Walkthroughs](sysman-state-walk.md)