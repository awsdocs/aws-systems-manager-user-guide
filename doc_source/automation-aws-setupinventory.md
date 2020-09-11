# AWS\-SetupInventory<a name="automation-aws-setupinventory"></a>

**Description**

Create a Systems Manager Inventory association for one or more managed instances\. The system collects metadata from your instances according to the schedule in the association\. For more information, see [AWS Systems Manager Inventory](systems-manager-inventory.md)\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-SetupInventory)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ Applications

  Type: String

  Default: Enabled

  Description: \(Optional\) Collect metadata about installed applications\.
+ AssociatedDocName

  Type: String

  Default: AWS\-GatherSoftwareInventory

  Description: \(Optional\) The name of the SSM document used to collect Inventory from the managed instance\.
+ AssociationName

  Type: String

  Description: \(Optional\) A name for the Inventory association that will be assigned to the instance\.
+ AssocWaitTime

  Type: String

  Default: PT5M

  Description: \(Optional\) Amount of time that Inventory collection should pause when the Inventory association start time is reached\. The time uses ISO 8601 format\.
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ AwsComponents

  Type: String

  Default: Enabled

  Description: \(Optional\) Collect metadata for AWS Components like amazon\-ssm\-agent\.
+ CustomInventory

  Type: String

  Default: Enabled

  Description: \(Optional\) Collect custom inventory metadata\.
+ Files

  Type: String

  Description: \(Optional\) Collect metadata about files on your instances\. For more information about how to collect this type of Inventory data, see [Working with file and Windows registry inventory](sysman-inventory-file-and-registry.md)\. Requires SSMAgent version 2\.2\.64\.0 or later\. Linux example: \[\{"Path":"/usr/bin", "Pattern":\["aws\*", "\*ssm\*"\],"Recursive":false\},\{"Path":"/var/log", "Pattern":\["amazon\*\.\*"\], "Recursive":true, "DirScanLimit":1000\}\] Windows example: \[\{"Path":"%PROGRAMFILES%", "Pattern":\["\*\.exe"\],"Recursive":true\}\]
+ InstanceDetailedInformation

  Type: String

  Default: Enabled

  Description: \(Optional\) Collect additional information about the instance, including the CPU model, speed, and the number of cores, to name a few\.
+ InstanceIds

  Type: String

  Default: \*

  Description: \(Required\) EC2 instances that you want to inventory\.
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf\. If not specified a transient role will be created to run the Lambda function\.
+ NetworkConfig

  Type: String

  Default: Enabled

  Description: \(Optional\) Collect metadata about network configurations\.
+ OutputS3BucketName

  Type: String

  Description: \(Optional\) Name of an Amazon S3 bucket where you want to write Inventory log data\.
+ OutputS3KeyPrefix

  Type: String

  Description: \(Optional\) An Amazon S3 key prefix \(subfolder\) where you want to write Inventory log data\.
+ OutputS3Region

  Type: String

  Description: \(Optional\) The name of the AWS Region where the Amazon S3 exists\.
+ Schedule

  Type: String

  Default: cron\(0 \*/30 \* \* \* ? \*\)

  Description: \(Optional\) A cron expression for the Inventory association schedule\. The default is every 30 minutes\.
+ Services

  Type: String

  Default: Enabled

  Description: \(Optional, Windows OS only, requires SSMAgent version 2\.2\.64\.0 and above\) Collect data for service configurations\.
+ WindowsRegistry

  Type: String

  Description: \(Optional\) Collect metadata about Microsoft Windows Registry keys\. For more information about how to collect this type of Inventory data, see [Working with file and Windows registry inventory](sysman-inventory-file-and-registry.md)\. Requires SSM Agent version 2\.2\.64\.0 or later\. Example: \[ \{"Path":"HKEY\_CURRENT\_CONFIG\\System","Recursive":true\},\{"Path":"HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Amazon\\MachineImage", "ValueNames":\["AMIName"\]\}\]
+ WindowsRoles

  Type: String

  Default: Enabled

  Description: \(Optional\) Collect information about Windows roles on the instance\. Applies to Windows operating systems only\. Requires SSMAgent version 2\.2\.64\.0 or later\.
+ WindowsUpdates

  Type: String

  Default: Enabled

  Description: \(Optional\) Collect data about all Windows Updates on the instance\.