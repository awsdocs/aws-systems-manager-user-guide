# Systems Manager prerequisites<a name="systems-manager-prereqs"></a>

The prerequisites for using AWS Systems Manager to manage your EC2 instances, on\-premises servers, and virtual machines \(VMs\) are covered step by step in the *Setting Up* chapters of this user guide:
+ [Setting up AWS Systems Manager](systems-manager-setting-up.md)
+ [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)

This topic provides an overview of these prerequisites\. 

**To complete prerequisites for using Systems Manager**

1. Create an AWS account and configure the required IAM roles\.

1. Verify that Systems Manager is supported in the AWS Regions where you want to use the service\.

1. Verify that your instances run a supported [operating system](https://docs.aws.amazon.com/systems-manager/latest/userguide/prereqs-operating-systems.html)\.

1. For EC2 instances, create an IAM instance profile and attach it to your machines\.

1. For on\-premises servers and VMs, create an IAM service role for a hybrid environment\.

1. Verify that you are allowing HTTPS \(port 443\) outbound traffic to the Systems Manager endpoints\.

1. \(Recommended\) Create a VPC endpoint in Amazon Virtual Private Cloud to use with Systems Manager\. 

1. On on\-premises servers, VMs, and EC2 instances created from AMIs that are not supplied by AWS, install a Transport Layer Security \(TLS\) certificate\.

1. For on\-premises servers and VMs, register the machines with Systems Manager through the managed instance activation process\.

1. Install or verify installation of SSM Agent on each of your managed instances\.

**Integration with IAM and Amazon EC2**  
User access to Systems Manager, its capabilities, and its resources are controlled through policies that you use or create in **AWS Identity and Access Management \(IAM\)**\. If you plan to use computing resources provided by AWS, and not only on\-premises servers and virtual machines \(VMs\), you also need to understand **Amazon Elastic Compute Cloud \(Amazon EC2\)** before you set up Systems Manager for your organization\. Understanding how these services work is essential to successfully set up Systems Manager\.

For more information about Amazon EC2, see the following:
+ [Amazon Elastic Compute Cloud \(Amazon EC2\)](https://aws.amazon.com/ec2/)
+ [Getting Started with Amazon EC2 Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)
+ [Getting Started with Amazon EC2 Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html)
+  [What is Amazon EC2?](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) \(Linux\)
+ [What is Amazon EC2?](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/concepts.html) \(Windows\)

For more information about IAM, see the following:
+ [AWS Identity and Access Management \(IAM\)](https://aws.amazon.com/iam/)
+ [Getting Started with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started.html)
+ [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/)