# Setting up AWS Systems Manager for EC2 instances<a name="systems-manager-setting-up-ec2"></a>

Complete the tasks in this section to set up and configure roles, user accounts, permissions, and initial resources for AWS Systems Manager\. The tasks described in this section are typically performed by AWS account and systems administrators\. After these steps are complete, users in your organization can use Systems Manager to configure, manage, and access Amazon Elastic Compute Cloud \(Amazon EC2\) instances\.

**Note**  
If you plan to use Systems Manager to manage and configure on\-premises machines, follow the setup steps in [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)\. If you plan to use both Amazon EC2 instances *and* your own computing resources in a hybrid environment, follow the steps here first\. This section presents steps in the recommended order for configuring the roles, users, permissions, and initial resources to use in your Systems Manager operations\. 

If you already use other AWS services, you have completed some of these steps\. However, other steps are specific to Systems Manager\. Therefore, we recommend reviewing this entire section to ensure that you're ready to use all Systems Manager capabilities\. 

**Topics**
+ [Step 1: Complete general Systems Manager setup steps](systems-manager-ec2-setup-general.md)
+ [Step 2: Create non\-Admin IAM users and groups for Systems Manager](setup-create-iam-user.md)
+ [Step 3: Create an IAM instance profile for Systems Manager](setup-instance-profile.md)
+ [Step 4: Attach an IAM instance profile to an Amazon EC2 instance](setup-launch-managed-instance.md)
+ [Step 5: Create VPC endpoints](setup-create-vpc.md)