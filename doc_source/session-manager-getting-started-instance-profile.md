# Step 2: Verify or create an IAM role with Session Manager permissions<a name="session-manager-getting-started-instance-profile"></a>

By default, AWS Systems Manager doesn't have permission to perform actions on your instances\. You must grant access by using AWS Identity and Access Management \(IAM\)\. For Amazon Elastic Compute Cloud \(Amazon EC2\) instances, permissions are provided by an instance profile\. An instance profile passes an IAM role to an Amazon EC2 instance\. You can attach an IAM instance profile to an Amazon EC2 instance as you launch it or to a previously launched instance\. For more information, see [Using instance profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-usingrole-instanceprofile.html)\.

For on\-premises servers or virtual machines \(VMs\), permissions are provided by the IAM service role associated with the hybrid activation used to register your on\-premises servers and VMs with Systems Manager\. On\-premises servers and VMs do not use instance profiles\.

If you already use other Systems Manager capabilities, such as Run Command or Parameter Store, an instance profile with the required basic permissions for Session Manager might already be attached to your Amazon EC2 instances\. If an instance profile that contains the AWS managed policy `AmazonSSMManagedInstanceCore` is already attached to your instances, the required permissions for Session Manager are already provided\. This is also true if the IAM service role used in your hybrid activation contains the `AmazonSSMManagedInstanceCore` managed policy\.

**Important**  
You can't change the IAM service role associated with a hybrid activation\. If you find that the service role does not contain the required permissions, you must deregister the managed instance and register it with a new hybrid activation that uses a service role with the required permissions\. For more information about deregistering managed instances, see [Deregistering managed nodes in a hybrid environment](systems-manager-managed-instances-advanced-deregister.md)\. For more information about creating an IAM service role for on\-premises machines, see [Create an IAM service role for a hybrid environment](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-service-role.html)\.

However, in some cases, you might need to modify the permissions attached to your instance profile\. For example, you want to provide a narrower set of instance permissions, you have created a custom policy for your instance profile, or you want to use Amazon Simple Storage Service \(Amazon S3\) encryption or AWS Key Management Service \(AWS KMS\) encryption options for securing session data\. For these cases, do one of the following to allow Session Manager actions to be performed on your instances:
+  **Embed permissions for Session Manager actions in a custom IAM role** 

  To add permissions for Session Manager actions to an existing IAM role that doesn't rely on the AWS\-provided default policy `AmazonSSMManagedInstanceCore`, follow the steps in [Adding Session Manager permissions to an existing IAM role](getting-started-add-permissions-to-existing-profile.md)\.
+  **Create a custom IAM role with Session Manager permissions only** 

  To create an IAM role that contains permissions only for Session Manager actions, follow the steps in [Create a custom IAM role for Session Manager](getting-started-create-iam-instance-profile.md)\.
+  **Create and use a new IAM role with permissions for all Systems Manager actions** 

  To create an IAM role for Systems Manager managed instances that uses a default policy supplied by AWS to grant all Systems Manager permissions, follow the steps in [Configure instance permissions for Systems Manager](setup-instance-permissions.md)\.

**Topics**
+ [Adding Session Manager permissions to an existing IAM role](getting-started-add-permissions-to-existing-profile.md)
+ [Create a custom IAM role for Session Manager](getting-started-create-iam-instance-profile.md)