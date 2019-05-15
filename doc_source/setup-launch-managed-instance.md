# Step 5: Launch an Amazon EC2 Instance that Uses the Systems Manager Instance Profile<a name="setup-launch-managed-instance"></a>

This procedure describes how to launch an Amazon EC2 instance that uses the instance profile you created in the previous topic, [Step 4: Create an IAM Instance Profile for Systems Manager](setup-instance-profile.md)\. 

If you already have EC2 instances in your account that you want to use, you can instead attach the instance profile to those instances\. For more information, see [Attaching an IAM Role to an Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide*\.

**SSM Agent requirements for instances**  
AWS Systems Manager Agent \(SSM Agent\) is Amazon software that can be installed and configured on an Amazon EC2 instance, an on\-premises server, or a virtual machine \(VM\)\. SSM Agent makes it possible for Systems Manager to update, manage, and configure these resources\.

If the Amazon Machine Image \(AMI\) type you choose in this procedure doesn't come with SSM Agent preinstalled, you must manually install the agent on the instance before it can be used with Systems Manager\. SSM Agent is installed by default on the following AMIs:
+ Windows Server 2003\-2012 R2 AMIs published in November 2016 or later
+ Windows Server 2016 and 2019
+ Amazon Linux
+ Amazon Linux 2
+ Ubuntu Server 16\.04
+ Ubuntu Server 18\.04

For information about manually installing SSM Agent on other Linux operating systems, see [Installing and Configuring SSM Agent on Amazon EC2 Linux Instances](sysman-install-ssm-agent.md)\.

**TLS certificate requirement for instances**  
A Transport Layer Security \(TLS\) certificate must be installed on each managed instance you use with Systems Manager\. These certificates are used to encrypt calls to other AWS services\. A TLS certificate is already installed on each Amazon EC2 instance created from any Amazon Machine Image \(AMI\)\. On instances created from AMIs not supplied by Amazon, and on your own on\-premises servers and VMs, you must install the certificate yourself\. For more information, see [Prerequisite: TLS Certificates](systems-manager-prereqs.md#prereqs-tls-certificate)\.

**To create an instance that uses the Systems Manager instance profile \(console\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation bar at the top of the screen, the current AWS Region is displayed\. Select the [region](https://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) for the instance\.

1. Choose **Launch Instance**\.

1. On the **Choose an Amazon Machine Image \(AMI\)** page, locate the AMI for the instance type you want to create, and then choose **Select**\.

1. Choose **Next: Configure Instance Details**\.

1. On the **Configure Instance Details** page, in the **IAM role** drop\-down list, choose the instance profile you created using the procedure in [Step 4: Create an IAM Instance Profile for Systems Manager](setup-instance-profile.md)\.

1. Complete the wizard\.

   For more information, see one of the following topics, depending on your instance type:
   + **Linux**: [Launching an Instance Using the Launch Instance Wizard](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html) in the *Amazon EC2 User Guide for Linux Instances*
   + **Windows**: [Launching an Instance Using the Launch Instance Wizard](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/launching-instance.html) in the *Amazon EC2 User Guide for Windows Instances*

If you create other instances that you want to configure using Systems Manager, you must specify the instance profile for each instance\.

Continue to [Step 6: \(Optional\) Create a Virtual Private Cloud Endpoint](setup-create-vpc.md)\.