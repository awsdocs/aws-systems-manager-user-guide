# Getting started with Configuration Compliance<a name="sysman-compliance-prereqs"></a>

To get started with Configuration Compliance, complete the following tasks\.


****  

| Task | For more information | 
| --- | --- | 
|  Configuration Compliance works with Patch Manager patch data, State Manager associations, and custom compliance types on Systems Manager managed instances\. Verify that your EC2 instances and hybrid machines \(on\-premises instances and virtual machines \(VMs\)are configured as managed instances by verifying Systems Manager prerequisites\.  |  [Systems Manager prerequisites](systems-manager-prereqs.md)  | 
|  Update SSM Agent on your managed instances to the latest version\.  |  [Working with SSM Agent](ssm-agent.md)  | 
|  If you plan to monitor patch compliance, verify that you've configured Systems Manager Patch Manager\. You must perform patching operations by using Patch Manager before Configuration Compliance can display patch compliance data\.  |  [AWS Systems Manager Patch Manager](systems-manager-patch.md)  | 
|  If you plan to monitor association compliance, verify that you've created State Manager associations\. You must create associations before Configuration Compliance can display association compliance data\.  |  [AWS Systems Manager State Manager](systems-manager-state.md)  | 
|  \(Optional\) Configure the system to view compliance history and change tracking\.   |  [Viewing compliance configuration history and change tracking](sysman-compliance-about.md#sysman-compliance-history)  | 
|  \(Optional\) Create custom compliance types\.   |  [Configuration Compliance walkthrough \(AWS CLI\)](sysman-compliance-walk.md)  | 
|  \(Optional\) Create a Resource Data Sync to aggregate all compliance data in a target S3 bucket\.  |  [Creating a Resource Data Sync for Configuration Compliance](sysman-compliance-datasync-create.md)  | 