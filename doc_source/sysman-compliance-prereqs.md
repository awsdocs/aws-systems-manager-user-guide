# Getting started with Compliance<a name="sysman-compliance-prereqs"></a>

To get started with Compliance, a capability of AWS Systems Manager, complete the following tasks\.


****  

| Task | For more information | 
| --- | --- | 
|  Compliance works with patch data in Patch Manager and associations in State Manager\. \(Patch Manager and State Manager are also both capabilities of AWS Systems Manager\.\) Compliance also works with custom compliance types on managed nodes that are managed using Systems Manager\. Verify that your Amazon Elastic Compute Cloud \(Amazon EC2\) instances, edge devices, and on\-premises servers and virtual machines are configured as managed nodes by verifying Systems Manager prerequisites\.  |  [Systems Manager prerequisites](systems-manager-prereqs.md)  | 
|  Update Systems Manager SSM Agent \(SSM Agent\) on your managed nodes to the latest version\.  |  [Working with SSM Agent](ssm-agent.md)  | 
|  If you plan to monitor patch compliance, verify that you've configured Patch Manager\. You must perform patching operations by using Patch Manager before Compliance can display patch compliance data\.  |  [AWS Systems Manager Patch Manager](patch-manager.md)  | 
|  If you plan to monitor association compliance, verify that you've created State Manager associations\. You must create associations before Compliance can display association compliance data\.  |  [AWS Systems Manager State Manager](systems-manager-state.md)  | 
|  \(Optional\) Configure the system to view compliance history and change tracking\.   |  [Viewing compliance configuration history and change tracking](sysman-compliance-about.md#sysman-compliance-history)  | 
|  \(Optional\) Create custom compliance types\.   |  [Compliance walkthrough \(AWS CLI\)](sysman-compliance-walk.md)  | 
|  \(Optional\) Create a resource data sync to aggregate all compliance data in a target Amazon Simple Storage Service \(Amazon S3\) bucket\.  |  [Creating a resource data sync for Compliance](sysman-compliance-datasync-create.md)  | 