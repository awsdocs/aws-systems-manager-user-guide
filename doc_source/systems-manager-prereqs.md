# Systems Manager prerequisites<a name="systems-manager-prereqs"></a>

The prerequisites for using AWS Systems Manager to manage your Amazon Elastic Compute Cloud \(Amazon EC2\) instances, edge devices, on\-premises servers, and VMs are covered step by step in the *Setting Up* chapters of this user guide:
+ [Setting up AWS Systems Manager](systems-manager-setting-up.md)
+ [Setting up Systems Manager for hybrid environments](systems-manager-managedinstances.md)
+ [Setting up AWS Systems Manager for edge devices](systems-manager-setting-up-edge-devices.md)

This topic provides an overview of these prerequisites\. 

**To complete prerequisites for using Systems Manager**

1. Configure the required AWS Identity and Access Management \(IAM\) roles in your AWS account\.

1. Verify that Systems Manager is supported in the AWS Regions where you want to use the service\.

1. Verify that your machines run a supported [operating system](https://docs.aws.amazon.com/systems-manager/latest/userguide/prereqs-operating-systems.html)\.

1. For edge devices, verify that your devices are configured to run the AWS IoT Greengrass Core software\. For edge devices that don't run AWS IoT Greengrass Core software, the machines must be configured as on\-premises machines for Systems Manager\.

1. For Amazon EC2 instances, create an IAM instance profile and attach it to your machines\.

1. For on\-premises servers, edge devices, and VMs, create an IAM service role\.

1. \(Recommended\) Create a VPC endpoint in Amazon Virtual Private Cloud \(Amazon VPC\) to use with Systems Manager\. 

   If you don't use a VPC endpoint, configure your managed instances to allow `HTTPS` \(port 443\) outbound traffic to the Systems Manager endpoints\. For information, see [Create VPC endpoints](https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-create-vpc.html)\.

1. For on\-premises servers, edge devices, VMs, and Amazon EC2 instances created from Amazon Machine Images \(AMIs\) that aren't supplied by AWS, ensure that a Transport Layer Security \(TLS\) certificate is installed\.

1. For on\-premises servers and VMs, register the machines with Systems Manager through the managed instance activation process\.

1. Install or verify installation of the SSM Agent on each of your managed nodes\.

1. For Amazon EC2 instances, verify the instance can reach the Instance Metadata Service \(IMDS\)\. Systems Manager relies on EC2 instance metadata to function correctly\.

**Note**  
SSM Agent initiates all connections to the Systems Manager service in cloud\. For this reason, you don't need to configure your firewall to allow inbound traffic to your managed nodes for Systems Manager\.  
If your managed nodes don't display in Systems Manager after you've follow these steps, see [Troubleshooting managed node availability](troubleshooting-managed-instances.md)\.

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