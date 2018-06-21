# Task 3: Create an Amazon EC2 Instance that Uses the Systems Manager Instance Profile<a name="sysman-create-instance-with-role"></a>

This procedure describes how to launch an Amazon EC2 instance that uses the instance profile you created in the previous topic, [Task 2: Create an Instance Profile for Systems Manager](sysman-configuring-access-role.md)\. You can instead attach the instance profile to an existing instance\. For more information, see [Attaching an IAM Role to an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide*\.

**To create an instance that uses the Systems Manager instance profile**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation bar at the top of the screen, the current region is displayed\. Select the [region](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) for the instance\.

1. Choose **Launch Instance**\.

1. On the **Choose an Amazon Machine Image \(AMI\)** page, locate the AMI for the instance type you want to create, and then choose **Select**\.

1. Choose **Next: Configure Instance Details**\.

1. On the **Configure Instance Details** page, in the **IAM role** drop\-down list, choose the instance profile you created using the steps in [Task 2: Create an Instance Profile for Systems Manager](sysman-configuring-access-role.md)\.

1. Complete the wizard\.

   For more information, see one of the following topics, depending on your instance type:
   + **Linux**: [Launching an Instance Using the Launch Instance Wizard](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html) in the *Amazon EC2 User Guide for Linux Instances*
   + **Windows**: [Launching an Instance Using the Launch Instance Wizard](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/launching-instance.html) in the *Amazon EC2 User Guide for Windows Instances*

If you create other instances that you want to configure using Systems Manager, you must specify the instance profile for each instance\.