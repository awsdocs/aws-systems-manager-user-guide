# What is AWS Systems Manager?<a name="what-is-systems-manager"></a>

AWS Systems Manager is an AWS service that you can use to view and control your infrastructure on AWS\. Using the Systems Manager console, you can view operational data from multiple AWS services and automate operational tasks across your AWS resources\. Systems Manager helps you maintain security and compliance by scanning your *managed instances* and reporting on \(or taking corrective action on\) any policy violations it detects\.

A managed instance is a machine that has been configured for use with Systems Manager\. Systems Manager also helps you configure and maintain your managed instances\. Supported machine types include EC2 instances, on\-premises servers, and virtual machines \(VMs\), including VMs in other cloud environments\. Supported operating system types include Windows Server, multiple distributions of Linux, and Raspbian\.

Using Systems Manager, you can associate AWS resources together by applying the same identifying *resource tag* to each of them\. You can then view operational data for these resources as a *resource group*, to help monitor and troubleshoot\. 

For example, you can assign a resource tag of "`Operation=North Region OS Patching`" to all of the following resources:
+ A group of EC2 instances
+ A group of on\-premises servers in your own facility
+ A Systems Manager patch baseline that specifies which patches to apply to your managed instances
+ An S3 bucket to store patching operation log output
+ A Systems Manager *maintenance window* that specifies the schedule for the patching operation

After tagging the resources, you can view a consolidated dashboard in Systems Manager that reports the status of all the resources that are part of the patching operation in your North region\. If a problem arises with any of these resources, you can take corrective action immediately\. 

**Capabilities in Systems Manager**  
Systems Manager is comprised of individual *[capabilities](features.md)*, which are grouped into five categories: *Operations Management*, *Application Management*, *Actions & Change*, *Instances & Nodes*, and *Shared Resources*\.

This collection of capabilities is a powerful set of tools and features that you can use to perform many operational tasks\. For example:
+ Group AWS resources together by any purpose or activity you choose, such as application, environment, region, project, campaign, business unit, or software lifecycle\.
+ Centrally define the configuration options and policies for your managed instances\.
+ Centrally view, investigate, and resolve operational work items related to AWS resources\.
+ Automate or schedule a variety of maintenance and deployment tasks\.
+ Use and create runbook\-style *SSM documents* that define the actions to perform on your managed instances\.
+ Run a command, with rate and error controls, that targets an entire fleet of managed instances\.
+ Securely connect to a managed instance with a single click, without having to open an inbound port or manage SSH keys\.
+ Separate your secrets and configuration data from your code by using *parameters*, with or without encryption, and then reference those parameters from a number of other AWS services\.
+ Perform automated inventory by collecting metadata about your Amazon EC2 and on\-premises managed instances\. Metadata can include information about applications, network configurations, and more\.
+ View consolidated inventory data from multiple AWS Regions and accounts that you manage\.
+ Quickly see which resources in your account are out of compliance and take corrective action from a centralized dashboard\.
+ View active summaries of metrics and alarms for your AWS resources\.

Systems Manager simplifies resource and application management, shortens the time to detect and resolve operational problems, and helps you operate and manage your AWS infrastructure securely at scale\. 

**Note**  
AWS Systems Manager was formerly known as *Amazon Simple Systems Manager \(SSM\)* and *Amazon EC2 Systems Manager \(SSM\)*\. For more information, see [Systems Manager Service Name History](#service-naming-history)\.

**Video: What is AWS Systems Manager?**  
View a video introduction to Systems Manager \(Duration: 1:42\)

[![AWS Videos](http://img.youtube.com/vi/MK4ZoCs-muo/0.jpg)](http://www.youtube.com/watch?v=MK4ZoCs-muo)

View more AWS videos on the [Amazon Web Services YouTube Channel](https://www.youtube.com/user/AmazonWebServices)\.

**Systems Manager Supported Regions**  
AWS Systems Manager is available in the AWS Regions listed in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\. Before starting your Systems Manager configuration process, we recommend that you ensure the service is available in each of the AWS Regions you want to use it in\.   
For on\-premises servers and VMs in your hybrid environment, we recommend that you choose the Region closest to your data center or computing environment\.

**Systems Manager Pricing**  
Some Systems Manager capabilities charge a fee\. For more information, see [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\.

**Systems Manager Service Name history**  
AWS Systems Manager \(Systems Manager\) was formerly known as "Amazon Simple Systems Manager \(SSM\)" and "Amazon EC2 Systems Manager \(SSM\)"\. The original abbreviated name of the service, "SSM", is still reflected in various AWS resources, including a few other service consoles\. Some examples:  
+ **Systems Manager Agent**: SSM Agent
+ **Systems Manager parameters**: SSM parameters
+ **Systems Manager service endpoints**: `ssm.us-east-2.amazonaws.com`
+ **AWS CloudFormation resource types**: `AWS::SSM::Document`
+ **AWS Config rule identifier**: `EC2_INSTANCE_MANAGED_BY_SSM`
+ **AWS CLI commands**: `aws ssm describe-patch-baselines`
+ **AWS Identity and Access Management \(IAM\) managed policy names**: `AmazonSSMReadOnlyAccess`
+ **Systems Manager resource ARNs**: `arn:aws:ssm:us-east-2:111222333444:patchbaseline/pb-07d8884178EXAMPLE`

**Related content**
+ The following resources can help you work directly with Systems Manager\.
  + **[AWS Blog & Podcast](http://aws.amazon.com/blogs/)** – Read blog posts about Systems Manager in the [AWS Management Tools Category](http://aws.amazon.com/blogs/aws/category/management-tools/amazon-ec2-systems-manager/), as well as other posts that are tagged with [http://aws.amazon.com/blogs/mt/tag/aws-systems-manager/](http://aws.amazon.com/blogs/mt/tag/aws-systems-manager/)\.
  + **[Systems Manager Developer Forum](https://forums.aws.amazon.com/forum.jspa?forumID=185)** – Follow announcements, or post or answer a question in the AWS Systems Manager Forum\.
  + **[AWS Systems Manager API Reference](https://docs.aws.amazon.com/systems-manager/latest/APIReference/)** – Provides descriptions, syntax, and usage examples for each of the Systems Manager actions and data types\.
  + **[AWS Systems Manager section of the AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/ssm/index.html)** – Manage Systems Manager from a command line tool\. Available to use on Windows, Mac, and Linux/UNIX systems\.
  + **[AWS Systems Manager section of the AWS Tools for PowerShell Cmdlet Reference](https://docs.aws.amazon.com/powershell/latest/reference/items/AWS_Systems_Manager_cmdlets.html)** – Manage Systems Manager with the same PowerShell tools that you use to manage your Windows, Linux, or Mac environments\. 
  + **[Systems Manager service quotas](https://docs.aws.amazon.com/general/latest/gr/ssm.html#limits_ssm) in the *Amazon Web Services General Reference*** – Provides the default quotas for Systems Manager for an AWS account\. Unless otherwise noted, each quota is Region\-specific\.

  The following related resources can help you as you work with this service\.
  + ** [Classes & Workshops](https://aws.amazon.com/training/course-descriptions/)** – Links to role\-based and specialty courses as well as self\-paced labs to help sharpen your AWS skills and gain practical experience\.
  + ** [AWS Developer Tools](https://aws.amazon.com/tools/)** – Links to developer tools, SDKs, IDE toolkits, and command line tools for developing and managing AWS applications\.
  + ** [AWS Whitepapers](https://aws.amazon.com/whitepapers/)** – Links to a comprehensive list of technical AWS whitepapers, covering topics such as architecture, security, and economics and authored by AWS Solutions Architects or other technical experts\.
  + ** [AWS Support Center](https://console.aws.amazon.com/support/home#/)** – The hub for creating and managing your AWS Support cases\. Also includes links to other helpful resources, such as forums, technical FAQs, service health status, and AWS Trusted Advisor\.
  + ** [AWS Support](https://aws.amazon.com/premiumsupport/)** – The primary web page for information about AWS Support, a one\-on\-one, fast\-response support channel to help you build and run applications in the cloud\.
  + ** [Contact Us](https://aws.amazon.com/contact-us/)** – A central contact point for inquiries concerning AWS billing, account, events, abuse, and other issues\. 
  + ** [AWS Site Terms](https://aws.amazon.com/terms/)** – Detailed information about our copyright and trademark; your account, license, and site access; and other topics\.

**Topics**
+ [Systems Manager capabilities](features.md)
+ [How Systems Manager works](how-it-works.md)
+ [About SSM Agent](prereqs-ssm-agent.md)
+ [Supported operating systems](prereqs-operating-systems.md)
+ [Accessing Systems Manager](access-methods.md)
+ [Systems Manager prerequisites](systems-manager-prereqs.md)