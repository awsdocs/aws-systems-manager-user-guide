# Systems Manager prerequisites<a name="systems-manager-prereqs"></a>

The prerequisites for using AWS Systems Manager to manage your Amazon Elastic Compute Cloud \(Amazon EC2\) instances, on\-premises servers, and virtual machines \(VMs\) are covered step by step in the *Setting Up* chapters of this user guide:
+ [Setting up AWS Systems Manager](systems-manager-setting-up.md)
+ [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)

This topic provides an overview of these prerequisites\. 

**To complete prerequisites for using Systems Manager**

1. Create an AWS account and configure the required AWS Identity and Access Management \(IAM\) roles\.

1. Verify that Systems Manager is supported in the AWS Regions where you want to use the service\.

1. Verify that your instances run a supported [operating system](https://docs.aws.amazon.com/systems-manager/latest/userguide/prereqs-operating-systems.html)\.

1. For Amazon EC2 instances, create an IAM instance profile and attach it to your machines\.

1. For on\-premises servers and VMs, create an IAM service role for a hybrid environment\.

1. \(Recommended\) Create a VPC endpoint in Amazon Virtual Private Cloud \(Amazon VPC\) to use with Systems Manager\. 

   If you don't use a VPC endpoint, configure your managed instances to allow `HTTPS` \(port 443\) outbound traffic to the Systems Manager endpoints\. For information, see [\(Optional\) Create a Virtual Private Cloud endpoint](https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-create-vpc.html)\.

1. On on\-premises servers, VMs, and Amazon EC2 instances created from Amazon Machine Images \(AMIs\) that aren't supplied by AWS, ensure that a Transport Layer Security \(TLS\) certificate is installed\.

1. For on\-premises servers and VMs, register the machines with Systems Manager through the managed instance activation process\.

1. Install or verify installation of the SSM Agent on each of your managed instances\.

**Note**  
SSM Agent initiates all connections to the Systems Manager service in cloud\. For this reason, you don't need to configure your firewall to allow inbound traffic to your instances for Systems Manager\.  
If EC2 instances you have created aren't displaying in Systems Manager after you've follow these steps, see [Troubleshooting managed instance availability](troubleshooting-managed-instances.md)\.

**Integration with IAM and Amazon EC2**  
User access to Systems Manager, its capabilities, and its resources are controlled through policies that you use or create in AWS Identity and Access Management\. If you plan to use computing resources provided by AWS and on\-premises servers and virtual machines \(VMs\), you also need to understand Amazon Elastic Compute Cloud before you set up Systems Manager for your organization\. Understanding how these services work is essential to successfully set up Systems Manager\.

For more information about Amazon EC2, see the following:
+ [Amazon Elastic Compute Cloud](http://aws.amazon.com/ec2/)
+ [Getting Started with Amazon EC2 Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)
+ [Getting Started with Amazon EC2 Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html)
+  [What is Amazon EC2?](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) \(Linux\)
+ [What is Amazon EC2?](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/concepts.html) \(Windows\)
+ [Amazon EC2 Mac instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-mac-instances.html) in the *Amazon EC2 User Guide for Linux Instances*

For more information about IAM, see the following:
+ [AWS Identity and Access Management \(IAM\)](http://aws.amazon.com/iam/)
+ [Getting Started with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started.html)
+ [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/)