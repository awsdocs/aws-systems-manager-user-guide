# Setting up Systems Manager for EC2 instances<a name="systems-manager-setting-up-ec2"></a>

Complete the tasks in this section to set up and configure roles, permissions, and initial resources for AWS Systems Manager\. The tasks described in this section are typically performed by AWS account and systems administrators\. After these steps are complete, users in your organization can use Systems Manager to configure, manage, and access Amazon Elastic Compute Cloud \(Amazon EC2\) instances\.

**Note**  
If you plan to use Systems Manager to manage and configure on\-premises machines, follow the setup steps in [Setting up Systems Manager for hybrid environments](systems-manager-managedinstances.md)\. If you plan to use both Amazon EC2 instances *and* your own computing resources in a hybrid environment, follow the steps here first\. This section presents steps in the recommended order for configuring the roles, users, permissions, and initial resources to use in your Systems Manager operations\. 

If you already use other AWS services, you have completed some of these steps\. However, other steps are specific to Systems Manager\. Therefore, we recommend reviewing this entire section to ensure that you're ready to use all Systems Manager capabilities\. 

**Topics**
+ [Step 1: Configure instance permissions for Systems Manager](setup-instance-permissions.md)
+ [Step 2: Create VPC endpoints](setup-create-vpc.md)