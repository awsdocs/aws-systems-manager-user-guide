# Setting up AWS Systems Manager for EC2 instances<a name="systems-manager-setting-up-ec2"></a>

Complete the tasks in this section to setup and configure roles, user accounts, permissions, and initial resources for AWS Systems Manager\. The tasks described in this section are typically performed by AWS account and systems administrators\. After these steps are complete, users in your organization can use Systems Manager to configure and manage Amazon Elastic Compute Cloud \(Amazon EC2\) and on\-premises servers and virtual machines \(VMs\) in a *hybrid environment*\.

If you plan to use Systems Manager to manage and configure on\-premises machines, follow the setup steps in [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)\. If you plan to use Amazon EC2 instances *and* your own computing resources in a hybrid environment, follow the steps in [Setting up AWS Systems Manager for EC2 instances](#systems-manager-setting-up-ec2)\. If you plan to use both Amazon EC2 instances *and* your own computing resources in a hybrid environment, follow the steps here first\. This section presents steps in the best order for configuring the roles, users, permissions, and initial resources to use in your Systems Manager operations\. 

If you already use other AWS services, you have completed some of these steps\. However, other steps are specific to Systems Manager\. Therefore, we recommend reviewing this entire section to ensure that you're ready to use all Systems Manager capabilities\. 

**Topics**
+ [Step 1: Sign up for AWS](setup-sign-up.md)
+ [Step 2: Create an Admin IAM user for AWS](setup-create-admin-user.md)
+ [Step 3: Create non\-Admin IAM users and groups for Systems Manager](setup-create-iam-user.md)
+ [Step 4: Create an IAM instance profile for Systems Manager](setup-instance-profile.md)
+ [Step 5: Attach an IAM instance profile to an Amazon EC2 instance](setup-launch-managed-instance.md)
+ [Step 6: \(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md)
+ [Step 7: \(Optional\) Create Systems Manager service roles](setup-service-role.md)
+ [Step 8: \(Optional\) Set up integrations with other AWS services](setup-integrations.md)