# AWSEC2\-SQLServerDBRestore<a name="automation-awsec2-sqlserverdbrestore"></a>

**Description**

The `AWSEC2-SQLServerDBRestore` document restores Microsoft SQL Server database backups stored in Amazon S3 to SQL Server 2017 running on an Amazon Elastic Compute Cloud \(EC2\) Linux instance\. You may provide your own EC2 instance running SQL Server 2017 Linux\. If an EC2 instance is not provided, the automation workflow launches and configures a new Ubuntu 16\.04 EC2 instance with SQL Server 2017\. The automation supports restoring full, differential, and transactional log backups\. This automation accepts multiple database backup files and automatically restores the most recent valid backup of each database in the files provided\.

To automate both backup and restore of an on\-premises SQL Server database to an EC2 instance running SQL Server 2017 Linux, you can use the AWS\-signed PowerShell script [samples/MigrateSQLServerToEC2Linux.zip](samples/MigrateSQLServerToEC2Linux.zip)\. 

**Important**  
This automation workflow resets the SQL Server server administrator \(SA\) user password every time the workflow runs\. After the automation workflow is complete, you must set your own SA user password again before you connect to the SQL Server instance\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSEC2-SQLServerDBRestore)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Linux

**Prerequisites**
+ This Automation document only works with EC2 instances for Linux running SQL Server\.
+ This Automation workflow must be run by a user with, at minimum, the permissions designated in the **Required IAM Permissions** section below\.
+ If you are providing your own EC2 instance:
  + Configure the EC2 instance with an AWS Identity and Access Management \(IAM\) instance profile that has the `AmazonSSMManagedInstanceCore` managed policy attached\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.
  + Verify that SSM Agent is installed on your EC2 instance\. For more information, see [Installing and configuring SSM Agent on EC2 instances for Linux](sysman-install-ssm-agent.md)\.
  + Verify that the EC2 instance has enough free disk space to download and restore the SQL Server backups\.

**Limitations**

This automation does not support restoring to SQL Server running on EC2 instances for Windows Server\. This automation only restores database backups that are compatible with SQL Server Linux 2017\. For more information, see [Editions and Supported Features of SQL Server 2017 on Linux](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-editions-and-components-2017?view=sql-server-2017)\.

**Parameters**
+ DatabaseNames

  Type: String

  Description: \(Optional\) Comma\-separated list of the names of databases to restore\.
+ DataDirectorySize

  Type: String

  Description: \(Optional\) Desired volume size \(GiB\) of the SQL Server Data directory for the new EC2 instance\.

  Default value: 100
+ KeyPair

  Type: String

  Description: \(Optional\) Key pair to use when creating the new EC2 instance\.
+ IamInstanceProfileName

  Type: String

  Description: \(Optional\) The IAM instance profile to attach to the new EC2 instance\. The IAM instance profile must have the `AmazonSSMManagedInstanceCore` managed policy attached\.
+ InstanceId

  Type: String

  Description: \(Optional\) The instance running SQL Server 2017 on Linux\. If no InstanceId is provided, the automation launches a new EC2 instance using the InstanceType and SQLServerEdition provided\.
+ InstanceType

  Type: String

  Description: \(Optional\) The instance type of the EC2 instance to be launched\.
+ IsS3PresignedUrl

  Type: String

  Description: \(Optional\) If S3Input is a pre\-signed S3 URL, indicate `yes`\.

  Default value: no

  Valid values: yes \| no 
+ LogDirectorySize

  Type: String

  Description: \(Optional\) Desired volume size \(GiB\) of the SQL Server Log directory for the new EC2 instance\.

  Default value: 100
+ S3Input

  Type: String

  Description: \(Required\) S3 bucket name, comma\-separated list of S3 object keys, or comma\-separated list of pre\-signed S3 URLs containing the SQL backup files to be restored\.
+ SQLServerEdition

  Type: String

  Description: \(Optional\) The edition of SQL Server 2017 to be installed on the newly created EC2 instance\.

  Valid values: Standard \| Enterprise \| Web \| Express
+ SubnetId

  Type: String

  Description: \(Optional\) The subnet in which to launch the new EC2 instance\. The subnet must have outbound connectivity to AWS services\. If a value for SubnetId is not provided, the automation uses the default subnet\.
+ TempDbDirectorySize

  Type: String

  Description: \(Optional\) Desired volume size \(GiB\) of the SQL Server TempDB directory for the new EC2 instance\.

  Default value: 100

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeImages",
                "ec2:RunInstances",
                "ec2:CreateTags",
                "ec2:DescribeInstances",
                "ec2:DescribeInstanceStatus",
                "ec2:RebootInstances",
                "ssm:SendCommand",
                "ssm:GetAutomationExecution",
                "ssm:ListCommands",
                "ssm:StartAutomationExecution",
                "ssm:DescribeInstanceInformation",
                "ssm:ListCommandInvocations",
                "iam:PassRole"
            ],
            "Resource": "*"
        }
    ]
}
```

**Document Steps**

**For new EC2 instances:**

1. aws:executeAwsApi \- Retrieve the AMI ID for SQL Server 2017 on Ubuntu 16\.04\.

1. aws:runInstances \- Launch a new EC2 instance for Linux\.

1. aws:waitForAwsResourceProperty \- Wait for the newly created EC2 instance to be ready\.

1. aws:executeAwsApi \- Reboot the instance if the instance is not ready\.

1. aws:assertAwsResourceProperty \- Verify that SSM Agent is installed\.

1. aws:runCommand \- Run the SQL Server restore script in PowerShell\.

**For existing EC2 instances:**

1. aws:waitForAwsResourceProperty \- Verify that the EC2 instance is ready\.

1. aws:executeAwsApi \- Reboot the instance if the instance is not ready\.

1. aws:assertAwsResourceProperty \- Verify that SSM Agent is installed\.

1. aws:runCommand \- Run the SQL Server restore script in PowerShell\.

**Outputs**

getInstance\.InstanceId

restoreToNewInstance\.Output

restoreToExistingInstance\.Output