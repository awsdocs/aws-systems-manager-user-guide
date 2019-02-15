# AWS Systems Manager State Manager<a name="systems-manager-state"></a>

AWS Systems Manager State Manager is a secure and scalable configuration management service that automates the process of keeping your Amazon EC2 and hybrid infrastructure in a state that you define\.

The following list describes the types of tasks you can perform with State Manager\.
+ Bootstrap instances with specific software at start\-up
+ Download and update agents on a defined schedule, including SSM Agent
+ Configure network settings
+ Join instances to a Windows domain \(Windows instances only\)
+ Patch instances with software updates throughout their lifecycle
+ Run scripts on Linux and Windows managed instances throughout their lifecycle

State Manager integrates with AWS CloudTrail to provide a record of all executions that you can audit, and Amazon CloudWatch Events to track state changes\. You can also choose to store and view detailed command output in Amazon S3\.

**Getting Started with State Manager**

Complete the following tasks to get started with State Manager\.


****  

| Task | For More Information | 
| --- | --- | 
|  Verify Systems Manager prerequisites  |  [Systems Manager Prerequisites](systems-manager-prereqs.md)  | 
|  Learn more about State Manager  |  [About State Manager](sysman-state-about.md)  | 
|  Create and assign a State Manager association to your instances  |  [Create an Association \(Console\)](sysman-state-assoc.md)  | 

**Related Content**  
See the following blog posts for additional examples of how to use State Manager:
+ [Combating Configuration Drift Using Amazon EC2 Systems Manager and Windows PowerShell DSC](https://aws.amazon.com/blogs/mt/combating-configuration-drift-using-amazon-ec2-systems-manager-and-windows-powershell-dsc/)
+ [Running Ansible Playbooks using Amazon EC2 Systems Manager Run Command and State Manager](https://aws.amazon.com/blogs/mt/running-ansible-playbooks-using-ec2-systems-manager-run-command-and-state-manager/)
+ [Configure Amazon EC2 Instances in an Auto Scaling Group Using State Manager](https://aws.amazon.com/blogs/mt/configure-amazon-ec2-instances-in-an-auto-scaling-group-using-state-manager/)

**Topics**
+ [About State Manager](sysman-state-about.md)
+ [Working with Associations in Systems Manager](systems-manager-associations.md)
+ [Systems Manager State Manager Walkthroughs](sysman-state-walk.md)