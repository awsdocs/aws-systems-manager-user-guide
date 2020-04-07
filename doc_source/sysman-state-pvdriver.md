# Walkthrough: Automatically Update PV Drivers on EC2 Windows Instances \(Console\)<a name="sysman-state-pvdriver"></a>

Amazon Windows AMIs contain a set of drivers to permit access to virtualized hardware\. These drivers are used by Amazon EC2 to map instance store and Amazon EBS volumes to their devices\. We recommend that you install the latest drivers to improve stability and performance of your EC2 Windows instances\. For more information about PV drivers, see [AWS PV Drivers](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/xen-drivers-overview.html#xen-driver-awspv)\.

The following walkthrough shows you how to configure a State Manager association to automatically download and install new AWS PV drivers when the drivers become available\.

**Before You Begin**  
Before you complete the following procedure, verify that you have at least one EC2 Windows instance running that is configured for Systems Manager\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\. 

**To create a State Manager association that automatically updates PV drivers**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)\. 

1. In the navigation pane, expand **Systems Manager Services**, and then choose **State Manager**\.

1. Choose **Create Association**\.

1. In the **Association Name** field, enter a descriptive name\.

1. In the **Select Document** list, choose **AWS\-ConfigureAWSPackage**\.

1. In **Select Targets by**, choose an option\.
**Note**  
If you choose to target instances by using tags, and you specify tags that map to Linux instances, the association succeeds on the Windows instance, but fails on the Linux instances\. The overall status of the association shows **Failed**\.

1. In **Schedule**, choose an option\. Updated PV drivers are released only a few times a year, so you can schedule the association to run once a month, if you want\.

1. In **Parameters**, choose **Install** from the **Action** list\.

1. For **Name**, enter **AWSPVDriver**\. You can leave the **Version** field empty\.

1. In **Advanced**, choose **Write to S3** if you want to write association details to an S3 bucket\.

1. Ignore **S3Region**\. This field is deprecated\. In the **S3Bucket Name** field, enter the name of your bucket\. If want to write output to a subfolder, specify the subfolder name in **S3Key Prefix**\. 

1. Choose **Create Association**, and then choose **Close**\. The system attempts to create the association on the instance or instances and immediately apply the state\. The association status shows **Pending**\.

1. In the right corner of the **Association** page, choose the refresh button\. If you created the association on one or more EC2 Windows instances, the status changes to **Success**\. If your instances are not properly configured for Systems Manager, or if you inadvertently targeted Linux instances, the status shows **Failed**\.

1. If the status is **Failed**, choose the **Instances** tab and verify that the association was successfully created on your EC2 Windows instances\. If Windows instances show a status of **Failed**, verify that SSM Agent is running on the instance, and verify that the instance is configured with an IAM role for Systems Manager\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.