# Step 5: Attach an IAM instance profile to an EC2 instance<a name="setup-launch-managed-instance"></a>

The procedures in this topic describe how to attach the IAM instance profile for Systems Manager that you created in the previous topic, [Step 4: Create an IAM instance profile for Systems Manager](setup-instance-profile.md), to EC2 instances\. You can attach the instance profile to new EC2 instances when you launch them, or to existing EC2 instances\. 

**SSM Agent requirements for instances**  
AWS Systems Manager Agent \(SSM Agent\) is Amazon software that can be installed and configured on an EC2 instance, an on\-premises server, or a virtual machine \(VM\)\. SSM Agent makes it possible for Systems Manager to update, manage, and configure these resources\.

If the Amazon Machine Image \(AMI\) type you choose in the first procedure doesn't come with SSM Agent preinstalled, you must manually install the agent on the new instance before it can be used with Systems Manager\. If SSM Agent isn't installed on the existing EC2 instance you choose in the second procedure, you must manually install the agent on the instance before it can be used with Systems Manager\.

SSM Agent is installed by default on the following AMIs:
+ Windows Server 2008\-2012 R2 AMIs published in November 2016 or later
+ Windows Server 2016 and 2019
+ Amazon Linux
+ Amazon Linux 2
+ Ubuntu Server 16\.04
+ Ubuntu Server 18\.04
+ Amazon ECS\-Optimized

For information about manually installing SSM Agent on other Linux operating systems, see [Installing and configuring SSM Agent on EC2 instances for Linux](sysman-install-ssm-agent.md)\.

**TLS certificate requirement for instances**  
A Transport Layer Security \(TLS\) certificate must be installed on each managed instance you use with Systems Manager\. These certificates are used to encrypt calls to other AWS services\. A TLS certificate is already installed on each EC2 instance created from any Amazon Machine Image \(AMI\)\. On instances created from AMIs not supplied by Amazon, and on your own on\-premises servers and VMs, you must install the certificate yourself\. For more information, see [Install a TLS certificate on on\-premises servers and VMs](hybrid-tls-certificate.md)\.

**Topics**
+ [Launch an instance that uses the Systems Manager instance profile \(console\)](#setup-launch-managed-instance-new)
+ [Attach the Systems Manager instance profile to an existing instance \(console\)](#setup-launch-managed-instance-existing)

## Launch an instance that uses the Systems Manager instance profile \(console\)<a name="setup-launch-managed-instance-new"></a>

**To launch an instance that uses the Systems Manager instance profile \(console\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation bar at the top of the screen, select the AWS Region for the instance\.

1. Choose **Launch Instance**\.

1. On the **Choose an Amazon Machine Image \(AMI\)** page, locate the AMI for the instance type you want to create, and then choose **Select**\.

1. Choose the type of instance to launch, such as **t2\.micro**, and then choose **Next: Configure Instance Details**\.

1. On the **Configure Instance Details** page, in the **IAM role** drop\-down list, select the instance profile you created using the procedure in [Step 4: Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.

1. For other options on the page, make selections that meet your requirements for the instance\. For more information, choose one of the following, depending on your selected operating system type:
   + **Linux**: [Launching an Instance Using the Launch Instance Wizard](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html) in the *Amazon EC2 User Guide for Linux Instances*
   + **Windows Server**: [Launching an Instance Using the Launch Instance Wizard](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/launching-instance.html) in the *Amazon EC2 User Guide for Windows Instances*

1. Complete the wizard\.

If you create other instances that you want to configure using Systems Manager, you must specify the instance profile for each instance\.

## Attach the Systems Manager instance profile to an existing instance \(console\)<a name="setup-launch-managed-instance-existing"></a>

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Instances**, choose **Instances**\.

1. Browse to and choose your EC2 instance from the list\.

1. In the **Actions** menu, choose **Instance Settings**, **Attach/Replace IAM Role**\.

1. For **IAM role**, select the instance profile you created using the procedure in [Step 4: Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.

1. Choose **Apply**\.

For more information about attaching IAM roles to instances, see [Attaching an IAM Role to an Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide for Linux Instances*\.

Continue to [Step 6: \(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md)\.