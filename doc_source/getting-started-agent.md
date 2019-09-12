# Step 3: Practice Installing or Updating SSM Agent on an Instance<a name="getting-started-agent"></a>

AWS Systems Manager Agent \(SSM Agent\) is Amazon software that can be installed and configured on an Amazon EC2 instance, an on\-premises server, or a virtual machine \(VM\)\. SSM Agent makes it possible for Systems Manager to update, manage, and configure these resources\. The agent processes requests from the Systems Manager service in the AWS Cloud, and then runs them as specified in the request\. SSM Agent then sends status and execution information back to the Systems Manager service by using the Amazon Message Delivery Service \(service prefix: `ec2messages`\)\.

In this task, you install or update the SSM Agent on an Amazon EC2 instance\.

**Note**  
If you are working with your own on\-premises servers or VMs, see the following topics:  
[Install SSM Agent for a Hybrid Environment \(Windows\)](sysman-install-managed-win.md) 
[Install SSM Agent for a Hybrid Environment \(Linux\)](sysman-install-managed-linux.md)

**Prerequisites**  
An instance profile for Systems Manager must already be attached to the Amazon EC2 instance that you update\. Refer to the following topics as needed to meet this requirement:
+ Create an EC2 instance profile for Systems Manager: [Create an IAM Instance Profile for Systems Manager](setup-instance-profile.md)
+ Attach the instance profile to an EC2 instance when you create the instance: [Attach an IAM Instance Profile to an Amazon EC2 Instance](setup-launch-managed-instance.md)
+ Attach the instance profile to an existing EC2 instance: [Attaching an IAM Role to an Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide*

**Windows Server instance**  
To practice installing or updating SSM Agent on an Amazon EC2 instance running Windows Server instance, follow the steps in [Install and Configure SSM Agent on Amazon EC2 Windows Instances](sysman-install-win.md)\.

**Linux instance**  
To practice installing or updating SSM Agent on an Amazon EC2 instance running Linux, follow the steps for your Linux operating system type in [Manually Install SSM Agent on Amazon EC2 Linux Instances](sysman-manual-agent-install.md)\.