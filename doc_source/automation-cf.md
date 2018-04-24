# Method 1: Use AWS CloudFormation to Configure Roles for Automation<a name="automation-cf"></a>

You can create an IAM instance profile role and a service role for Automation from an AWS CloudFormation template\.

After you create the instance profile role, you must assign it to any instance that you plan to configure using Automation\. For information about how to assign the role to an existing instance, see [Attaching an IAM Role to an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide*\. For information about how to assign the role when you create a new instance, see [Task 3: Create an Amazon EC2 Instance that Uses the Systems Manager Instance Profile](sysman-create-instance-with-role.md) in the **Configuring Access to Systems Manager** topic\.

**Note**  
You can also use these roles and their Amazon Resource Names \(ARNs\) in Automation documents, such as the AWS\-UpdateLinuxAmi document\. Using these roles or their ARNs in Automation documents enables Automation to perform actions on your managed instances, launch new instances, and perform actions on your behalf\. To view an example, see [Automation CLI Walkthrough: Patch a Linux AMI](automation-cliwalk.md)\.

**Topics**
+ [Create the Instance Profile Role and Service Role Using AWS CloudFormation](#automation-cf-create)
+ [Copy Role Information for Automation](#automation-cf-copy)

## Create the Instance Profile Role and Service Role Using AWS CloudFormation<a name="automation-cf-create"></a>

Use the following procedure to create the required IAM roles for Systems Manager Automation by using AWS CloudFormation\.

**To create the required IAM roles**

1. Choose the **Launch Stack** button\. The button opens the AWS CloudFormation console and populates the **Specify an Amazon S3 template URL** field with the URL to the Systems Manager Automation template\.
**Note**  
Choose **View** to view the template\.  
****    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/automation-cf.html)

1. Choose **Next**\.

1. On the **Specify Details** page, in the **Stack Name** field, either choose to keep the default value or specify your own value\. Choose **Next**\.

1. On the **Options** page, you don’t need to make any selections\. Choose **Next**\.

1. On the **Review** page, scroll down and choose the **I acknowledge that AWS CloudFormation might create IAM resources** option\.

1. Choose **Create**\.

AWS CloudFormation shows the **CREATE\_IN\_PROGRESS** status for approximately three minutes\. The status changes to **CREATE\_COMPLETE** after the stack has been created and your roles are ready to use\.

## Copy Role Information for Automation<a name="automation-cf-copy"></a>

Use the following procedure to copy information about the instance profile role and Automation service role from the AWS CloudFormation console\. You must specify these roles when you run an Automation document\.

**Note**  
You do not need to copy role information using this procedure if you run the AWS\-UpdateLinuxAmi or AWS\-UpdateWindowsAmi documents\. These documents already have the required roles specified as default values\. The roles specified in these documents use IAM managed policies\. 

**To copy the role names**

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Choose the check\-box beside the Automation stack you created in the previous procedure\.

1. Choose the **Resources** tab\.

1. The **Resources** table includes three items in the **Logical ID** column: **AutomationServiceRole**, **ManagedInstanceProfile**, and **ManagedInstanceRole**\.

1. Copy the **Physical ID** for **ManagedInstanceProfile**\. The physical ID will be similar to **Automation\-ManagedInstanceProfile\-1a2b3c4**\. This is the name of your instance profile role\.

1. Paste the instance profile role into a text file to use later\.

1. Choose the **Physical ID** link for **AutomationServiceRole**\. The IAM console opens to a summary of the Automation Service Role\.

1. Copy the Amazon Resource Name \(ARN\) beside **Role ARN**\. The ARN is similar to the following: arn:aws:iam::12345678:role/Automation\-AutomationServiceRole\-1A2B3C4D5E 

1. Paste the ARN into a text file to use later\.

You have finished configuring the required roles for Automation\. You can now use the instance profile role and the Automation service role ARN in your Automation documents\. For more information, see [Automation Console Walkthrough: Patch a Linux AMI](automation-consolewalk.md) and [Automation CLI Walkthrough: Patch a Linux AMI](automation-cliwalk.md)\.