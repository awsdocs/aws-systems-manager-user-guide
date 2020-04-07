# Setting Up AWS Systems Manager<a name="systems-manager-setting-up"></a>

This section describes the tasks that account and system administrators perform to set up AWS Systems Manager for their organizations\. After these steps are complete, users in the organization can use Systems Manager to configure and manage the Amazon Elastic Compute Cloud \(Amazon EC2\) instances in their account\.

If you plan to use Systems Manager to manage and configure your own on\-premises servers and virtual machines \(VMs\) in what is called a *hybrid environment*, follow the setup steps in [Setting Up AWS Systems Manager for Hybrid Environments](systems-manager-managedinstances.md)\. If you plan to use both Amazon EC2 instances *and* your own computing resources in a hybrid environment, follow the steps here first\. This section presents steps in the best order for configuring the roles, users, permissions, and initial resources to use in your Systems Manager operations\. 

If you already use other AWS services, you have completed some of these steps\. However, other steps are specific to Systems Manager\. Therefore, we recommend reviewing this entire section to ensure that you are ready to use all Systems Manager capabilities\. 

**Note**  
You can use Systems Manager Quick Setup to quickly configure an AWS Identity and Access Management \(IAM\) instance profile on all instances in your AWS account\. Quick Setup can also create an assume role, which enables Systems Manager to securely run commands on your instances on your behalf\. By using Quick Setup, you can skip step 4 and 5 in this section\. For more information, see [AWS Systems Manager Quick Setup](systems-manager-quick-setup.md)\. 

**Topics**
+ [Step 1: Sign Up for AWS](setup-sign-up.md)
+ [Step 2: Create an Admin IAM User for AWS](setup-create-admin-user.md)
+ [Step 3: Create Non\-Admin IAM Users and Groups for Systems Manager](setup-create-iam-user.md)
+ [Step 4: Create an IAM Instance Profile for Systems Manager](setup-instance-profile.md)
+ [Step 5: Attach an IAM Instance Profile to an Amazon EC2 Instance](setup-launch-managed-instance.md)
+ [Step 6: \(Optional\) Create a Virtual Private Cloud Endpoint](setup-create-vpc.md)
+ [Step 7: \(Optional\) Create Systems Manager Service Roles](setup-service-role.md)
+ [Step 8: \(Optional\) Set Up Integrations with Other AWS Services](setup-integrations.md)