# Task 2: Create an Instance Profile for Systems Manager<a name="sysman-configuring-access-role"></a>

By default, Systems Manager doesn't have permission to perform actions on your instances\. You must grant access by using an IAM instance profile\. An instance profile is a container that passes IAM role information to an Amazon EC2 instance at launch\. 

**Note**  
If you are configuring servers or virtual machines \(VMs\) in a hybrid environment for Systems Manager, you don't need to create an instance profile for them\. Instead, you must configure your servers and VMs to use an IAM service role\. For more information, see [Create an IAM Service Role for a Hybrid Environment](sysman-service-role.md)\.

You can create an instance profile for Systems Manager by attaching an AWS\-managed policy that defines the needed permissions to a new role or to a role you have already created\.

After you create the instance profile, you attach it to the instances that you want to use Systems Manager with\. To attach the instance profile to existing instances, see [Using Instance Profiles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html)\. To attach an instance profile to new instances when you create them, see the next topic, [Task 3: Create an Amazon EC2 Instance that Uses the Systems Manager Instance Profile](sysman-create-instance-with-role.md)\.

**Note**  
If you change the IAM instance profile, it might take some time for the instance credentials to refresh\. The SSM Agent will not process requests until this happens\. To speed up the refresh process, you can restart SSM Agent or restart the instance\. 

Depending on whether you will create a new role for your instance profile or add the needed permissions to an existing role, use one of the following procedures\.<a name="setup-instance-profile-managed-policy"></a>

**Create an instance profile for Systems Manager managed instances**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Select type of trusted entity** page, under **AWS Service**, choose **EC2**\.
**Note**  
If the **Select your use case** section appears, choose **EC2 Role for Simple Systems Manager**, and then choose **Next: Permissions**\.

1. On the **Attached permissions policy** page, verify that **AmazonEC2RoleforSSM** is listed, and then choose **Next: Review**\. 

1. On the **Review** page, type a name in the **Role name** box, and then type a description\.
**Note**  
Make a note of the role name\. You will choose this role when you create new instances that you want to manage by using Systems Manager\.

1. Choose **Create role**\. The system returns you to the **Roles** page\.<a name="setup-instance-profile-custom-policy"></a>

**Add instance profile permissions for Systems Manager managed instances to an existing role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose the existing role you want to associate with an instance profile for Systems Manager operations\.

1. On the **Permissions** tab, choose **Attach policy**\.

1. On the **Attach policy** page, select the check box next to AmazonEC2RoleforSSM, and then choose **Attach policy**\.

For information about how to update a role to include a trusted entity or further restrict access, see [Modifying a Role](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_manage_modify.html)\. 

For information about how to attach the role you just created to a new instance or an existing instance, see the next topic, [Task 3: Create an Amazon EC2 Instance that Uses the Systems Manager Instance Profile](sysman-create-instance-with-role.md)\.