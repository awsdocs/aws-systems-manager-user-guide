# How Systems Manager works<a name="how-it-works"></a>

AWS Systems Manager helps you manage, access, and troubleshoot AWS resources, including Amazon Elastic Compute Cloud \(Amazon EC2\) instances, edge devices, and on\-premises servers and virtual machines \(VMs\) in a hybrid environment\. The following diagram describes how Systems Manager capabilities, such as Session Manager and Patch Manager, perform actions on your resources\. Each enumerated interaction is described after the diagram\.

**Diagram 1: General example of Systems Manager process flow**

![\[Diagram showing how Systems Manager capabilities, for example Run Command or Maintenance Windows, use a similar process of set up, launching, processing, and reporting.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/how-it-works.png)

1. **Access Systems Manager** – You can access Systems Manager in the AWS Management Console\. If you prefer to manage resources programmatically, you can use the AWS Command Line Interface, AWS Tools for Windows PowerShell, or the AWS SDK\.

1. **Choose a Systems Manager capability** – Do you need to apply operating system patches to a fleet of Linux or Windows Server managed nodes? Do you want to connect to an Amazon EC2 instance using a secure, interactive, browser\-based shell? Systems Manager consists of more than two dozen [capabilities](https://docs.aws.amazon.com/systems-manager/latest/userguide/features.html) to help you perform actions on your resources\. The diagram shows only a few of the capabilities that administrators use to configure and manage their resources\.

1. **Verification and processing** – Systems Manager verifies that your AWS Identity and Access Management \(IAM\) user, group, or role has permission to perform the actions you specified on the designated resources\. After successfully verifying your permissions, the system sends your request to the AWS Systems Manager agent \(SSM Agent\) running on your managed nodes\. SSM Agent performs the specified configuration changes or actions\.

1. **Reporting** – SSM Agent reports the status of the configuration changes and actions to Systems Manager in the AWS Cloud, Systems Manager operations management capabilities, and various AWS services, if configured\.

1. **Systems Manager operations management capabilities** – If enabled, Systems Manager operations management capabilities such as Explorer OpsCenter, and Incident Manager aggregate operations data or create artifacts such as operational work items \(OpsItems\) and incidents in response to events or errors with your resources\. You can use these capabilities to help you investigate and troubleshoot problems\.