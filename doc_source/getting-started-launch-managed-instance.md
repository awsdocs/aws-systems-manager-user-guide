# Getting started with AWS Systems Manager<a name="getting-started-launch-managed-instance"></a>

Use this tutorial to get started with AWS Systems Manager\. You'll learn how to launch an Amazon Elastic Compute Cloud \(Amazon EC2\) instance that is managed by Systems Manager, and how to connect to the managed instance\.

Because Systems Manager is a collection of multiple capabilities, no single walkthrough or tutorial can introduce the entire service\. This tutorial provides an introduction to some of the capabilities\.

## Prerequisites<a name="getting-started-prerequisites"></a>

Before you begin, be sure that you've completed the steps in [Setting up Systems Manager for EC2 instances](systems-manager-setting-up-ec2.md)\.

## Launch an instance using an AMI with SSM Agent preinstalled<a name="getting-started-launch-instance-ami-preinstalled-agent"></a>

You can launch an Amazon EC2 instance using the AWS Management Console as described in the following procedure\. This tutorial is intended to help you launch your first managed instance quickly, so it doesn't cover all possible options\. 

**To launch an instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. From the EC2 console dashboard, in the **Launch instance** box, choose **Launch instance**, and then choose **Launch instance** from the options that appear\.

1. For **Name and tags**, for **Name**, enter a descriptive name for your instance\.

1. For **Application and OS Images \(Amazon Machine Image\)**, do the following:

   1. Choose the **Quick Start** tab, and then choose Amazon Linux\. This is the operating system \(OS\) for your instance\.

   1. For **Amazon Machine Image \(AMI\)**, choose an HVM version of Amazon Linux 2\.

1. For **Instance type**, from the **Instance type** list, choose the hardware configuration for your instance\. Choose the `t2.micro` instance type, which is selected by default\. The `t2.micro` instance type is eligible for the AWS Free Tier\. In AWS Regions where `t2.micro` is unavailable, you can use a `t3.micro` instance under the Free Tier\. For more information, see [AWS Free Tier](https://aws.amazon.com/free/)\.

1. For **Key pair \(login\)**, for **Key pair name**, choose a key pair\.

1. For **Network settings**, choose **Edit**\. For **Security group name**, notice that the wizard created and selected a security group for you\. You can use this security group, or alternatively you can select a security group that you created previously using the following steps:

   1. Choose **Select existing security group**\.

   1. From **Common security groups**, choose your security group from the list of existing security groups\.

1. If you aren't using Default Host Management Configuration, expand the **Advanced details** section, and for **IAM instance profile**, choose the instance profile that you created when getting set up in [Step 1: Configure instance permissions for Systems Manager](setup-instance-permissions.md)\.

1. Keep the default selections for the other configuration settings for your instance\.

1. Review a summary of your instance configuration in the **Summary** pane\. When you're ready, choose **Launch instance**\.

1. A confirmation page informs you that your instance is launching\. Choose **View all instances** to close the confirmation page and return to the console\.

1. On the **Instances** screen, you can view the status of the launch\. It takes a short time for an instance to launch\.

1. It can take a few minutes for the instance to show as managed and be ready for you to connect to it\. To check that your instance passed its status checks, view this information in the **Status check** column\.

## Connect to your managed instance<a name="getting-started-connect-managed-instance"></a>

**To connect to your managed instance**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the instance that you want to connect to using RDP\.

1. In the **Node actions** menu, choose **Start terminal session**\.

1. Select **Connect**\.

## Clean up your instance<a name="getting-started-cleanup"></a>

If you're done working with the managed instance that you created for this tutorial, terminate it\. Terminating an instance effectively deletes it\.

**To terminate your instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Instances**\. In the list of instances, select the instance\.

1. Choose **Instance state**, **Terminate instance**\.

1. Choose **Terminate** when prompted for confirmation\.

   Amazon EC2 shuts down and terminates your instance\. After your instance is terminated, it remains visible on the console briefly, and then the entry is deleted automatically\. You can't remove the terminated instance from the console display yourself\.