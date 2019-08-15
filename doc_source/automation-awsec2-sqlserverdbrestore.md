# AWSEC2\-SQLServerDBRestore<a name="automation-awsec2-sqlserverdbrestore"></a>

**Description**

The `AWSEC2-SQLServerDBRestore` document restores Microsoft SQL Server database backups stored in Amazon S3 to SQL Server 2017 running on an Amazon Elastic Compute Cloud \(EC2\) Linux instance\. You may provide your own EC2 instance running SQL Server 2017 Linux\. If an EC2 instance is not provided, the automation workflow launches and configures a new Ubuntu 16\.04 EC2 instance with SQL Server 2017\. The automation supports restoring full, differential, and transactional log backups\. This automation accepts multiple database backup files and automatically restores the most recent valid backup of each database in the files provided\.

To automate both backup and restore of an on\-premises SQL Server database to an Amazon EC2 instance running SQL Server 2017 Linux, see the AWS\-signed PowerShell script [MigrateSQLServerToEC2Linux\.ps1](https://s3-us-west-1.amazonaws.com/awsec2-server-upgrade-prod/MigrateSQLServerToEC2Linux.ps1)\. 

**Important**  
This automation workflow resets the SQL Server server administrator \(SA\) user password every time the workflow runs\. After the automation workflow is complete, you must set your own SA user password again before you connect to the SQL Server instance\.

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Linux

**Prerequisites**
+ This Automation document only works with Linux EC2 instances running SQL Server\.
+ This Automation workflow must be run by a user with, at minimum, the permissions designated in the **Required IAM Permissions** section below\.
+ If you are providing your own EC2 instance:
  + Configure the EC2 instance with an AWS Identity and Access Management \(IAM\) instance profile that has the `AmazonSSMManagedInstanceCore` managed policy attached\. For more information, see [Create an IAM Instance Profile for Systems Manager](setup-instance-profile.md)\.
  + Verify that SSM Agent is installed on your EC2 instance\. For more information, see [Installing and Configuring SSM Agent on Amazon EC2 Linux Instances](sysman-install-ssm-agent.md)\.
  + Verify that the EC2 instance has enough free disk space to download and restore the SQL Server backups\.

**Limitations**

This automation does not support restoring to SQL Server running on EC2 Windows instances\. This automation only restores database backups that are compatible with SQL Server Linux 2017\. For more information, see [Editions and Supported Features of SQL Server 2017 on Linux](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-editions-and-components-2017?view=sql-server-2017)\.

**Required IAM Permissions**

The user who runs the Automation workflow must have the following permissions:

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

**Parameters**
+ S3Input

  Type: String

  Description: \(Required\) S3 bucket name, comma\-separated list of S3 object keys, or comma\-separated list of pre\-signed S3 URLs containing the SQL backup files to be restored\.
+ IsS3PresignedUrl

  Type: String

  Description: \(Optional\) If S3Input is a pre\-signed S3 URL, indicate “yes”\.

  Default value: "no"

  Allowed values: "yes", "no" 
+ InstanceId

  Type: String

  Description: \(Optional\) The instance running SQL Server 2017 on Linux\. If no InstanceId is provided, the automation launches a new Amazon EC2 instance using the InstanceType and SQLServerEdition provided\.
+ InstanceType

  Type: String

  Description: \(Optional\) The instance type of the EC2 instance to be launched\.
+ SQLServerEdition

  Type: String

  Description: \(Optional\) The edition of SQL Server 2017 to be installed on the newly created EC2 instance\.

  Allowed values: "Standard", "Enterprise", "Web", "Express"
+ SubnetId

  Type: String

  Description: \(Optional\) The subnet in which to launch the new EC2 instance\. The subnet must have outbound connectivity to AWS services\. If a value for SubnetId is not provided, the automation uses the default subnet\.
+ IamInstanceProfileName

  Type: String

  Description: \(Optional\) The IAM instance profile to attach to the new EC2 instance\. The IAM instance profile must have the `AmazonSSMManagedInstanceCore` managed policy attached\.
+ DataDirectorySize

  Type: String

  Description: \(Optional\) Desired volume size \(GiB\) of the SQL Server Data directory for the new EC2 instance\.

  Default value: 100
+ LogDirectorySize

  Type: String

  Description: \(Optional\) Desired volume size \(GiB\) of the SQL Server Log directory for the new EC2 instance\.

  Default value: 100
+ TempDbDirectorySize

  Type: String

  Description: \(Optional\) Desired volume size \(GiB\) of the SQL Server TempDB directory for the new EC2 instance\.

  Default value: 100
+ DatabaseNames

  Type: String

  Description: \(Optional\) Comma\-separated list of the names of databases to restore\.
+ KeyPair

  Type: String

  Description: \(Optional\) Key pair to use when creating the new EC2 instance\.

**Examples**

Start the automation using an existing EC2 instance with two databases\.

```
aws ssm start-automation-execution --document-name AWSEC2-RestoreSQLServer --parameters "InstanceId='i-1234567890abcdef0',S3Input='sample-bucket/sample-prefix',DatabaseNames='sample-database1, sample-database2'"
```

Start the automation and launch a new EC2 instance with the specified SQL Server edition\.

```
aws ssm start-automation-execution --document-name AWSEC2-RestoreSQLServer --parameters "InstanceType='m4.large',SQLServerEdition='Standard',S3Input='sample-bucket/sample-backup-1.bak, sample-bucket/sample-backup-2.bak',IamInstanceProfileName='sample-profile-name',KeyPair='sample-keypair-name'"
```

Start the automation and launch a new EC2 instance with the specified SQL Server edition using a presigned S3 URL\.

```
aws ssm start-automation-execution --document-name AWSEC2-RestoreSQLServer --parameters "InstanceType='m4.large',SQLServerEdition='Standard',S3Input='https://sample-bucket.s3.amazonaws.com/sample-backup.bak?abcdefg',IsS3PresignedUrl='yes',IamInstanceProfileName='sample-profile-name',DataDirectorySize=200,LogDirectorySize=100,TempDbDirectorySize=200"
```

Retrieve the execution output\.

```
aws ssm get-automation-execution --automation-execution-id ExecutionID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

**For new EC2 instances:**

1. aws:executeAwsApi \- Retrieve the AMI ID for SQL Server 2017 on Ubuntu 16\.04\.

1. aws:runInstances \- Launch a new EC2 Linux instance\.

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