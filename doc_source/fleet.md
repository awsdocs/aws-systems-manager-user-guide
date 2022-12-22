# AWS Systems Manager Fleet Manager<a name="fleet"></a>

Fleet Manager, a capability of AWS Systems Manager, is a unified user interface \(UI\) experience that helps you remotely manage your nodes running on AWS or on premises\. With Fleet Manager, you can view the health and performance status of your entire server fleet from one console\. You can also gather data from individual nodes to perform common troubleshooting and management tasks from the console\. This includes connecting to Windows instances using the Remote Desktop Protocol \(RDP\), viewing folder and file contents, Windows registry management, operating system user management, and more\. To get started with Fleet Manager, open the [Systems Manager console](https://console.aws.amazon.com/systems-manager/managed-instances)\. In the navigation pane, choose **Fleet Manager**\.

## Who should use Fleet Manager?<a name="fleet-who"></a>

Any AWS customer who wants a centralized way to manage their node fleet should use Fleet Manager\.

## How can Fleet Manager benefit my organization?<a name="fleet-benefits"></a>

Fleet Manager offers these benefits:
+ Perform a variety of common systems administration tasks without having to manually connect to your managed nodes\.
+ Manage nodes running on multiple platforms from a single unified console\.
+ Manage nodes running different operating systems from a single unified console\.
+ Improve the efficiency of your systems administration\.

## What are the features of Fleet Manager?<a name="fleet-features"></a>

Key features of Fleet Manager include the following:
+ **Access the Red Hat Knowledgebase Portal**

  Access binaries, knowledge\-shares, and discussion forums on the Red Hat Knowledgebase Portal through your Red Hat Enterprise Linux \(RHEL\) instances\.
+ **Managed node status** 

  View which managed instances are `running` and which are `stopped`\. For more information about stopped instances, see [Stop and start your instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Stop_Start.html) in the *Amazon EC2 User Guide for Linux Instances*\. For AWS IoT Greengrass core devices, you can view which are `online`, `offline`, or show a status of `Connection lost`\.
**Note**  
If you stopped your managed instance before July 12, 2021, it won't display the `stopped` marker\. To show the marker, start and stop the instance\.
+ **View instance information**

  View information about the folder and file data stored on the volumes attached to your managed instances, performance data about your instances in real\-time, and log data stored on your instances\.
+ **View edge device information**

  View the AWS IoT Greengrass Thing name for the device, the SSM Agent ping status and version, and more\.
+ **Manage accounts and registry**

  Manage operating system \(OS\) user accounts on your instances and registry on your Windows instances\.
+ **Control access to features**

  Control access to Fleet Manager features using AWS Identity and Access Management \(IAM\) policies\. With these policies, you can control which individual users or groups in your organization can use various Fleet Manager features, and which managed nodes they can manage\.

**Topics**
+ [Who should use Fleet Manager?](#fleet-who)
+ [How can Fleet Manager benefit my organization?](#fleet-benefits)
+ [What are the features of Fleet Manager?](#fleet-features)
+ [Getting started with Fleet Manager](fleet-getting-started.md)
+ [Working with Fleet Manager](fleet-working.md)