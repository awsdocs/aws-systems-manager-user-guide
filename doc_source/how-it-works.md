# How Systems Manager works<a name="how-it-works"></a>

The following diagram describes how AWS Systems Manager capabilities, such as Session Manager and Patch Manager, interact with different types of resources in different environments\. Each enumerated interaction is described after the diagram\.

**Diagram 1: General example of Systems Manager process flow**

![\[Diagram showing how Systems Manager capabilities, for example Run Command or Maintenance Windows, use a similar process of set up, launching, processing, and reporting.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/how-it-works.png)

1. **Access Systems Manager** – You can access Systems Manager in the AWS Management Console\. If you prefer to manage resources programmatically, you can use the AWS Command Line Interface, AWS Tools for Windows PowerShell, or the AWS SDK\. With Systems Manager, you can configure, schedule, automate, and run actions that you want to perform on your AWS resources and managed nodes\. AWS resources can include AWS Identity and Access Management \(IAM\) users, groups, and roles; AWS Lambda functions; Amazon EC2 Auto Scaling groups; and Amazon Simple Storage Service \(Amazon S3\) buckets, to name a few\. Managed nodes can include Amazon Elastic Compute Cloud \(Amazon EC2\) instances, edge devices, and on\-premises servers and virtual machines \(VMs\) in your hybrid environment\. 

1. **Choose a Systems Manager capability** – Systems Manager consists of more than two dozen [capabilities](https://docs.aws.amazon.com/https://docs.aws.amazon.com/systems-manager/latest/userguide/features.html) to help you perform actions on your resources\. The diagram shows only a few of the capabilities that adminstrators use to configure and manage their resources\.

1. **Verification and processing** – Systems Manager verifies the configurations, including permissions, and sends requests to the AWS Systems Manager agent \(SSM Agent\) running on your instances, edge devices, or servers and VMs in your hybrid environment\. SSM Agent performs the specified configuration changes\.

1. **Reporting** – SSM Agent reports the status of the configuration changes and actions to the user, Systems Manager in the AWS Cloud, Systems Manager operations management capabilities, and various AWS services, if configured\.

1. **Systems Manager operations management capabilities** – If enabled, Systems Manager operations management capabilities such as Explorer OpsCenter, and Incident Manager aggregate operations data or create artifacts such as operational work items \(OpsItems\) and incidents in response to events or errors with your resources\. You can use these capabilities to help you investigate and troubleshoot problems\.