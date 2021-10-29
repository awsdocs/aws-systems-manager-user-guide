# AWS Systems Manager Fleet Manager<a name="fleet"></a>

Fleet Manager, a capability of AWS Systems Manager, is a unified user interface \(UI\) experience that helps you remotely manage your server fleet running on AWS, or on premises\. With Fleet Manager, you can view the health and performance status of your entire server fleet from one console\. You can also gather data from individual instances to perform common troubleshooting and management tasks from the console\. This includes viewing directory and file contents, Windows registry management, operating system user management, and more\.

## How can Fleet Manager benefit my organization?<a name="fleet-benefits"></a>

Fleet Manager offers these benefits:
+ Perform a variety of common systems administration tasks without having to manually connect to your instances\.
+ Manage instances running on multiple platforms from a single unified console\.
+ Manage instances running different operating systems from a single unified console\.
+ Improve the efficiency of your systems administration\.

## What are the features of Fleet Manager?<a name="fleet-features"></a>

Key features of Fleet Manager include the following:
+ **Instance status** 

  View which instances are `running` and which instances are `stopped`\. For more information about stopped instances, see [Stop and start your instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Stop_Start.html) in the *Amazon EC2 User Guide for Linux Instances*\.
**Note**  
If you stopped your instance before July 12, 2021, it won't display the `stopped` marker\. To show the marker, start and stop the instance\.
+ **View instance information**

  View information about the directory and file data stored on the volumes attached to your managed instances, performance data about your instances in real\-time, and log data stored on your instances\.
+ **Manage accounts and registry**

  Manage operating system \(OS\) user accounts on your instances and registry on your Windows instances\.
+ **Control access**

  Control access to Fleet Manager features using AWS Identity and Access Management \(IAM\) policies\. With these policies, you can control which individual users or groups in your organization can use various Fleet Manager features, and which Amazon Elastic Compute Cloud \(Amazon EC2\) instances they can manage\.

## What is a managed instance?<a name="fleet-managed-instance"></a>

A *managed instance* is any machine configured for AWS Systems Manager\. You can configure Amazon EC2 instances or on\-premises machines in a hybrid environment as managed instances\. Systems Manager supports various distributions of Linux, including Raspberry Pi devices, macOS, and Microsoft Windows Server\.

For more information about managed instances, see [Managed Instances](managed_instances.md)\.

**Topics**
+ [How can Fleet Manager benefit my organization?](#fleet-benefits)
+ [What are the features of Fleet Manager?](#fleet-features)
+ [What is a managed instance?](#fleet-managed-instance)
+ [Getting started with Fleet Manager](fleet-getting-started.md)
+ [Working with Fleet Manager](fleet-working.md)