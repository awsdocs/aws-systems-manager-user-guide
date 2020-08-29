# Step 2: Verify or create an IAM instance profile with Session Manager permissions<a name="session-manager-getting-started-instance-profile"></a>

By default, AWS Systems Manager doesn't have permission to perform actions on your instances\. You must grant access by using an IAM instance profile\. An instance profile is a container that passes IAM role information to an EC2 instance at launch\. This requirement applies to permissions for all AWS Systems Manager capabilities, not only those specific to Session Manager\.

If you already use other Systems Manager capabilities, such as Run Command or Parameter Store, an instance profile with the required basic permissions for Session Manager might already be attached to your instances\. If an instance profile that contains the AWS managed policy **AmazonSSMManagedInstanceCore** is already attached to your instances, the required permissions for Session Manager are already provided\.

However, in some cases, you might need to modify the permissions attached to your instance profile\. For example, you want to provide a narrower set of instance permissions, you have created a custom policy for your instance profile, or you want to use Amazon S3 encryption or AWS KMS encryption options for securing session data\. For these cases, do one of the following to allow Session Manager actions to be performed on your instances:
+  **Embed permissions for Session Manager actions in a custom instance profile** 

  To add permissions for Session Manager actions to an existing IAM instance profile that does not rely on the AWS\-provided default policy **AmazonSSMManagedInstanceCore**, follow the steps in [Adding Session Manager permissions to an existing instance profile](getting-started-add-permissions-to-existing-profile.md)\.
+  **Create a custom IAM instance profile with Session Manager permissions only** 

  To create an IAM instance profile that contains permissions only for Session Manager actions, follow the steps in [Create a custom IAM instance profile for Session Manager](getting-started-create-iam-instance-profile.md)\.
+  **Create and use a new instance profile with permissions for all Systems Manager actions** 

  To create an IAM instance profile for Systems Manager managed instances that uses a default policy supplied by AWS to grant all Systems Manager permissions, follow the steps in [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.

**Note**  
You can attach an IAM instance profile to an EC2 instance as you launch it or to a previously launched instance\. For more information, see [Instance Profiles](https://docs.aws.amazon.com/IAM/latest/UserGuide/roles-usingrole-instanceprofile.html)\.

**Topics**
+ [Adding Session Manager permissions to an existing instance profile](getting-started-add-permissions-to-existing-profile.md)
+ [Create a custom IAM instance profile for Session Manager](getting-started-create-iam-instance-profile.md)