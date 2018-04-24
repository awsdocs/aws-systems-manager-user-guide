# Task 3: Create an Amazon EC2 Instance that Uses the Systems Manager Instance Profile<a name="sysman-create-instance-with-role"></a>

This procedure describes how to launch an Amazon EC2 instance that uses the instance profile you created in the previous topic, [Task 2: Create an Instance Profile for Systems Manager](sysman-configuring-access-role.md)\. You can instead attach the instance profile to an existing instance\. For more information, see [Attaching an IAM Role to an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide*\.

**To create an instance that uses the Systems Manager instance profile**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Select a supported [region](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region)\.

1. Choose **Launch Instance** and select an instance\.

1. Choose your instance type and then choose **Next: Configure Instance Details**\.

1. In the **IAM role** drop\-down list choose the EC2 instance profile you created earlier\.

1. Complete the wizard\.

If you create other instances that you want to configure using Systems Manager, you must specify the instance profile for each instance\.