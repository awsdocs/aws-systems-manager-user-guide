# Using Run Command to Take VSS\-Enabled Snapshots of EBS Volumes<a name="integration-vss"></a>

Using Run Command, you can take application\-consistent snapshots of all [Amazon Elastic Block Store \(Amazon EBS\)](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EBSVolumes.html) volumes attached to your Amazon EC2 Windows instances\. The snapshot process uses the Windows [Volume Shadow Copy Service \(VSS\)](https://technet.microsoft.com/en-us/library/ee923636(v=ws.10).aspx) to take image\-level backups of VSS\-aware applications, including data from pending transactions between these applications and the disk\. Furthermore, you don't need to shut down your instances or disconnect them when you need to back up all attached volumes\. 

There is no additional cost to use VSS\-enabled EBS snapshots\. You only pay for EBS snapshots created by the backup process\. For more information, see [How is my EBS snapshot bill calculated?](https://aws.amazon.com/premiumsupport/knowledge-center/ebs-snapshot-billing/)

## How It Works<a name="integration-vss-how"></a>

Here is how the process of taking application\-consistent, VSS\-enabled EBS snapshots works\.

1. You verify and configure Systems Manager prerequisites\.

1. You enter parameters for the AWSEC2\-CreateVssSnapshot SSM document and execute this document by using Run Command\. You can't create a VSS\-enabled EBS snapshot for a specific volume\. You can, however, specify a parameter to exclude the boot volume from the backup process\.

1. The VSS agent on your instance coordinates all ongoing I/O operations for running applications\. 

1. The system flushes all I/O buffers and temporarily pauses all I/O operations\. The pause lasts, at most, ten seconds\.

1. During the pause, the system creates snapshots of all volumes attached to the instance\.

1. The pause is lifted and I/O resumes operation\. 

1. The system adds all newly\-created snapshots to the list of EBS snapshots\. The system tags all VSS\-enabled EBS snapshots successfully created by this process with **AppConsistent:true**\. This tag helps you identify snapshots created by this process, as opposed to other processes\. If the system encounters an error, the snapshot created by this process does not include the **AppConsistent:true** tag\.

1. In the event that you need to restore from a snapshot, you can restore by using the standard EBS process of creating a volume from a snapshot, or you can restore all volumes to an instance by using a sample script, which is described later in this section\. 

## Before You Begin<a name="integration-vss-prereqs"></a>

Before you create VSS\-enabled EBS snapshots by using Run Command, review the following requirements and limitations, and complete the required tasks\. 
+ VSS\-enabled EBS snapshots are supported for instances running Windows Server 2008 R2 or later\. \(Windows Server 2008 R2 Core is currently not supported\.\) Verify that your instances meet all requirements for Amazon EC2 Windows\. For more information, see [Setting Up AWS Systems Manager](systems-manager-setting-up.md)\.
+ Update your instances to use SSM Agent version 2\.2\.58\.0 or later\. If you are using an older version of SSM Agent, you can update it by using Run Command\. For more information, see [Example: Update the SSM Agent](rc-console.md#rc-console-agentexample)\.
+ Systems Manager requires permission to perform actions on your instances\. You must configure each instance with an AWS Identity and Access Management \(IAM\) instance profile role for Systems Manager\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\.
+ Systems Manager needs permissions to create and tag VSS\-enabled EBS snapshots\. You can configure an IAM role that enables these permissions\. You must configure each instance with a role for creating and tagging snapshots\. For more information, see [Create an IAM Role for VSS\-Enabled Snapshots](#integration-vss-role) in **Set Up Tasks**\. 
**Note**  
If you don't want to assign the snapshot role to your instances, you can use Run Command and the pre\-defined AWSEC2\-ManageVssIO SSM document to temporarily pause I/O, create VSS\-enabled EBS snapshots, and restart I/O\. This process runs in the context of the user who executes the command\. If the user has sufficient permission to create and tag snapshots, then Systems Manager can create and tag VSS\-enabled EBS snapshots without the need for the additional IAM snapshot role on the instance\. Instances still must be configured with the instance profile role\. For more information, see [Creating VSS\-Enabled EBS Snapshots by Using the AWSEC2\-ManageVssIO SSM Document \(Advanced\)](#integration-vss-AWSEC2-ManageVssIO)\.
+ Systems Manager requires VSS components to be installed on your instances\. If you need to install the required VSS components, then you can download and run a VSS package for Systems Manager on your instances\. If you plan to use your own Microsoft licenses for VSS \(BYOL\), you still need to install the VSS components for Systems Manager\. For more information, see [Download and Install VSS Components for Systems Manager](#integration-vss-package) in **Set Up Tasks**\. 

**Topics**
+ [Create an IAM Role for VSS\-Enabled Snapshots](#integration-vss-role)
+ [Download and Install VSS Components for Systems Manager](#integration-vss-package)

### Create an IAM Role for VSS\-Enabled Snapshots<a name="integration-vss-role"></a>

This section includes a procedure for creating an IAM policy, and a separate procedure for creating an IAM role that uses the policy you create in the first procedure\. The policy enables Systems Manager to create snapshots, tags snapshots, and attach metadata like a device ID to the default snapshot tags the system creates\. 

**To create an IAM policy for VSS\-enable snapshots**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. On the **Create policy** page, choose the **JSON** tab, and then copy and past the following JSON policy into the text box\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "VisualEditor0",
               "Effect": "Allow",
               "Action": "ec2:CreateTags",
               "Resource": "arn:aws:ec2:*::snapshot/*"
           },
           {
               "Sid": "VisualEditor1",
               "Effect": "Allow",
               "Action": [
                   "ec2:DescribeInstances",
                   "ec2:CreateSnapshot"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. On the **Review Policy** page, type a name in the **Policy Name** field, and then choose **Create Policy**\. The system returns you to the IAM console\.

Use the following procedure to create an IAM role for VSS\-enabled snapshots\. This role includes policies for Amazon EC2 and Systems Manager\.

**To create an IAM role for VSS\-enabled EBS snapshots**

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Select type of trusted entity** page, under **AWS Service**, choose **EC2**\.

1. In the **Select your use case** section, choose **EC2**, and then choose **Next: Permissions**\.

1. On the **Attach permissions policy** page, choose the policy you just created, and then choose **Next: Review**\.

1. On the **Review** page, type a name in the **Role name** box, and then type a description\.

1. Choose **Create role**\. The system returns you to the **Roles** page\.

1. Choose the role you just created\. The role **Summary page opens**\.

1. Choose **Attach policy**\.

1. Search for and choose the **AmazonEC2RoleforSSM**\.

1. Choose **Attach policy**\.

1. Attach this role to the instances for which you want to create VSS\-enabled EBS snapshots\. For more information, see [Attaching an IAM Role to an Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) in the *Amazon EC2 User Guide*\.

### Download and Install VSS Components for Systems Manager<a name="integration-vss-package"></a>

Systems Manager requires VSS components to be installed on your instances\. Use the following procedure to configure a State Manager association that automatically downloads and installs the components by using the AwsVssComponents package\. The State Manager association will automatically download and install new versions of the package when they are published by AWS\. The package installs two components: a VSS requestor and a VSS provider\. The system copies these components to a directory on the instance, and then registers the provider DLL as a VSS provider\. 

If you don't want Systems Manager to automatically download and install new versions of this package when they become available, you can use Run Command and the AWS\-ConfigureAWSPackage SSM document to install the package on your instances, as described later in this section\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create a State Manager association that automatically downloads and installs the AwsVssComponents package \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose ** State Manager**\.

1. Choose **Create an Association**\.

1. \(Optional\) In the **Name** field, type a descriptive name\.

1. In the **Command Document** list, choose **AWS\-ConfigureAWSPackage**\.

1. In the **Parameters** section, from the **Action** menu, choose **Install**\.

1. In the **Name** field, type **AwsVssComponents**\.

1. In the **Version** field, verify that **latest** is auto\-populated\.

1. In the **Targets** section, identify the instances where you want to run this operation by specifying tags or selecting instances manually\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. In the **Specify schedule** section, choose an option\.

1. In the **Output options** section, if you want to save the command output to a file, select the **Write command output to an Amazon S3 bucket**\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\. 

1. Choose **Create Association**, and then choose **Close**\. The system attempts to create the association on the instances and immediately apply the state\. The association status shows **Pending**\.

1. In the right corner of the **Association** page, choose the refresh button\. If you created the association on one or more EC2 Windows instances, the status changes to **Success**\. If your instances are not properly configured for Systems Manager, the status shows **Failed**\.

1. If the status is **Failed**, verify that the SSM Agent is running on the instance, and verify that the instance is configured with an IAM role for Systems Manager\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.

**To create a State Manager association that automatically downloads and installs the AwsVssComponents package \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Services** in the navigation pane, and then choose **State Manager**\.

1. Choose **Create Association**\.

1. In the **Association Name** field, type a descriptive name\.

1. In the **Select Document** list, choose **AWS\-ConfigureAWSPackage**\.

1. In the **Select Targets by** section, choose an option and select the instances where you want to download and run the script\.

1. In the **Schedule** section, choose an option\.

1. In the **Parameters** section, choose **Install** from the **Action** list\.

1. From the **Name** field, type **AwsVssComponents**\.

1. In the **Version** field, verify that **latest** is auto\-populated\.

1. In the **Advanced** section, choose **Write to S3** if you want to write association details to an Amazon S3 bucket\.

1. Choose **Create Association**, and then choose **Close**\. The system attempts to create the association on the instances and immediately apply the state\. The association status shows **Pending**\.

1. In the right corner of the **Association** page, choose the refresh button\. If you created the association on one or more EC2 Windows instances, the status changes to **Success**\. If your instances are not properly configured for Systems Manager, the status shows **Failed**\.

1. If the status is **Failed**, verify that the SSM Agent is running on the instance, and verify that the instance is configured with an IAM role for Systems Manager\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.

#### Install the VSS Package by Using the AWS CLI<a name="integration-vss-package-cli"></a>

Use the following procedure to download and install the AwsVssComponents package on your instances by using Run Command from the AWS CLI\. The package installs two components: a VSS requestor and a VSS provider\. The system copies these components to a directory on the instance, and then registers the provider DLL as a VSS provider\.

**To install the VSS package by using the AWS CLI**

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon EC2 or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\. 

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Execute the following command to download and install the required VSS components for Systems Manager\.

   ```
   aws ssm send-command --document-name "AWS-ConfigureAWSPackage" --instance-ids "i-12345678" --parameters '{"action":["Install"],"name":["AwsVssComponents"]}'
   ```

#### Install the VSS Package by Using Tools for Windows PowerShell<a name="integration-vss-package-ps"></a>

Use the following procedure to download and install the AwsVssComponents package on your instances by using Run Command from the Tools for Windows PowerShell\. The package installs two components: a VSS requestor and a VSS provider\. The system copies these components to a directory on the instance, and then registers the provider DLL as a VSS provider\.

**To install the VSS package by using AWS Tools for Windows PowerShell**

1. Open AWS Tools for Windows PowerShell and execute the following command to specify your credentials\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.

   ```
   Set-AWSCredentials –AccessKey key_name –SecretKey key_name
   ```

1. Execute the following command to set the Region for your PowerShell session\. The example uses the us\-east\-2 Region\.

   ```
   Set-DefaultAWSRegion -Region us-east-2
   ```

1. Execute the following command to download and install the required VSS components for Systems Manager\.

   ```
   Send-SSMCommand -DocumentName AWS-ConfigureAWSPackage -InstanceId "$instance"-Parameter @{'action'='Install';'name'='AwsVssComponents'}
   ```

## Creating VSS\-enabled EBS snapshots<a name="integration-vss-creating"></a>

This section includes procedures for creating VSS\-enabled EBS snapshots by using the Amazon EC2 console, the AWS CLI, and Tools for Windows PowerShell\.

**Topics**
+ [Create VSS\-enabled EBS snapshots by Using the Console](#integration-vss-console)
+ [Create VSS\-enabled EBS snapshots by Using the AWS CLI](#integration-vss-cli)
+ [Create VSS\-enabled EBS snapshots by Using AWS Tools for Windows PowerShell](#integration-vss-ps)

### Create VSS\-enabled EBS snapshots by Using the Console<a name="integration-vss-console"></a>

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create VSS\-enabled EBS snapshots by using the console \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Run Command**\.

1. Choose **Run a Command**\.

1. In the **Command document** list, choose **AWSEC2\-CreateVssSnapshot**\.

1. In the **Targets** section, identify the instances where you want to run this operation by specifying tags or selecting instances manually\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. In the **Command parameters** section:

   1. Choose an option from the **Exclude Boot Volume** list\. Use this parameter to exclude boot volumes from the backup process\.

   1. In the **Description** field, type a description\. This description is applied to any snapshot created by this process \(optional, but recommended\)\.

   1. In the **Tags** field, type keys and values for tags that you want to apply to any snapshot created by this process\. Tags can help you locate, manage, and restore volumes from a list of snapshots\. By default, the system populates the tag parameter with a `Name` key\. For the value of this key, specify a name that you want to apply to snapshots created by this process\. If you want to specify additional tags, separate tags by using a semicolon\. For example, `Key=Environment,Value=Test`;`Key=User,Value=TestUser1`\.

      This parameter is optional, but we recommended that you tag snapshots\. By default, the systems tags snapshots with the device ID, and `AppConsistent` \(for indicating successful, application\-consistent VSS\-enabled EBS snapshots\)\.

1. In **Other parameters**:
   + In the **Comment** box, type information about this command\.
   + In **Timeout \(seconds\)**, specify the number of seconds for the system to wait before failing the overall command execution\. 

1. \(Optional\) In **Rate control**:
   + In **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by choosing Amazon EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document at the same time by specifying a percentage\.
   + In **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify 3 errors, then Systems Manager stops sending the command when the 4th error is received\. Instances still processing the command might also send errors\.

1. In the **Output options** section, if you want to save the command output to a file, select the **Write command output to an Amazon S3 bucket**\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\. 

1. In the **SNS Notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.

1. Choose **Run**\.

   If successful, the command populates the list of EBS snapshots with the new snapshots\. You can locate these snapshots in the list of EBS snapshots by searching for the tags you specified, or by searching for `AppConsistent`\. If the command execution failed, view the Systems Manager command output for details about why the execution failed\. If the command successfully completed, but a specific volume backup failed, you can troubleshoot the failure in the list of EBS volumes\.

   If the command failed and you are using Systems Manager with VPC endpoints, verify that you configured the **com\.amazonaws\.*region*\.ec2** endpoint\. Without the EC2 endpoint defined, the call to enumerate attached EBS volumes fails, which causes the Systems Manager command to fail\. For more information about setting up VPC endpoint with Systems Manager, see [Setting Up VPC Endpoints for Systems Manager](sysman-setting-up-vpc.md)\.
**Note**  
You can automate backups by creating a Maintenance Windows task that uses the AWSEC2\-CreateVssSnapshot SSM document\. For more information, see [Working with Maintenance Windows](sysman-maintenance-working.md)\.

**To create VSS\-enabled EBS snapshots by using the console \(Amazon EC2 Systems Manager\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run a Command**\.

1. In the **Command document** list, choose **AWSEC2\-CreateVssSnapshot**\.

1. In the **Targets** section, identify the instances where you want to run this operation by specifying tags or selecting instances manually\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Where Are My Instances?](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. \(Optional\) In **Rate control**:
   + In **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by choosing Amazon EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document at the same time by specifying a percentage\.
   + In **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify 3 errors, then Systems Manager stops sending the command when the 4th error is received\. Instances still processing the command might also send errors\.

1. In the **Exclude Boot Volume** list, choose an option\. Use this parameter to exclude boot volumes from the backup process\.

1. In the **Description** field, type a description\. This description is applied to any snapshot created by this process \(optional, but recommended\)\.

1. In the **Tags** field, type keys and values for tags that you want to apply to any snapshot created by this process\. Tags can help you locate, manage, and restore volumes from a list of snapshots\. By default, the system populates the tag parameter with a `Name` key\. For the value of this key, specify a name that you want to apply to snapshots created by this process\. If you want to specify additional tags, separate tags by using a semicolon\. For example, `Key=Environment,Value=Test`;`Key=User,Value=TestUser1`\.

   This parameter is optional, but we recommended that you tag snapshots\. By default, the systems tags snapshots with the device ID, and `AppConsistent` \(for indicating successful, application\-consistent VSS\-enabled EBS snapshots\)\.

1. In the **Comments** field, type information about this command\.

1. In the **Output options** section, if you want to save the command output to a file, select the **Write command output to an Amazon S3 bucket**\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\. 

1. In the **SNS Notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.

1. Choose **Run**\.

   If successful, the command populates the list of EBS snapshots with the new snapshots\. You can locate these snapshots in the list of EBS snapshots by searching for the tags you specified, or by searching for `AppConsistent`\. If the command execution failed, view the Systems Manager command output for details about why the execution failed\. If the command successfully completed, but a specific volume backup failed, you can troubleshoot the failure in the list of EBS volumes\.

   If the command failed and you are using Systems Manager with VPC endpoints, verify that you configured the **com\.amazonaws\.*region*\.ec2** endpoint\. Without the EC2 endpoint defined, the call to enumerate attached EBS volumes fails, which causes the Systems Manager command to fail\. For more information about setting up VPC endpoint with Systems Manager, see [Setting Up VPC Endpoints for Systems Manager](sysman-setting-up-vpc.md)\.
**Note**  
You can automate backups by creating a Maintenance Windows task that uses the AWSEC2\-CreateVssSnapshot SSM document\. For more information, see [Working with Maintenance Windows](sysman-maintenance-working.md)\.

### Create VSS\-enabled EBS snapshots by Using the AWS CLI<a name="integration-vss-cli"></a>

Use the following procedure to create VSS\-enabled EBS snapshots by using the AWS CLI\. When you execute the command, you can specify the following parameters:
+ Instance \(Required\): Specify one or more Amazon EC2 Windows instances\. You can either manually specify instances, or you can specify tags\.
+ Description \(Optional\): Specify details about this backup\.
+ Tags \(Optional\): Specify key\-value tag pairs that you want to assign to the snapshots\. Tags can help you locate, manage, and restore volumes from a list of snapshots\. By default, the system populates the tag parameter with a `Name` key\. For the value of this key, specify a name that you want to apply to snapshots created by this process\. You can also add custom tags to this list by using the following format: `Key=Environment,Value=Test`;`Key=User,Value=TestUser1`\.

  This parameter is optional, but we recommended that you tag snapshots\. By default, the systems tags snapshots with the device ID, and `AppConsistent` \(for indicating successful, application\-consistent VSS\-enabled EBS snapshots\)\.
+ Exclude Boot Volume \(Optional\): Use this parameter to exclude boot volumes from the backup process\.

**To create VSS\-enabled EBS snapshots by using the AWS CLI**

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon EC2 or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\. 

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Execute the following command to create VSS\-enabled EBS snapshots\.

   ```
   aws ssm send-command --document-name "AWSEC2-CreateVssSnapshot" --instance-ids "i-12345678" --parameters '{"ExcludeBootVolume":["False"],"description":["Description"],"tags":["Key=key_name,Value=tag_value"]}'
   ```

If successful, the command populates the list of EBS snapshots with the new snapshots\. You can locate these snapshots in the list of EBS snapshots by searching for the tags you specified, or by searching for `AppConsistent`\. If the command execution failed, view the command output for details about why the execution failed\.

You can automate backups by creating a Maintenance Windows task that uses the AWSEC2\-CreateVssSnapshot SSM document\. For more information, see [Working with Maintenance Windows](sysman-maintenance-working.md)\.

### Create VSS\-enabled EBS snapshots by Using AWS Tools for Windows PowerShell<a name="integration-vss-ps"></a>

Use the following procedure to create VSS\-enabled EBS snapshots by using the AWS Tools for Windows PowerShell\. When you execute the command, you can specify the following parameters:
+ Instance \(Required\): Specify one or more Amazon EC2 Windows instances\. You can either manually specify instances, or you can specify tags\.
+ Description \(Optional\): Specify details about this backup\.
+ Tags \(Optional\): Specify key\-value tag pairs that you want to assign to the snapshots\. Tags can help you locate, manage, and restore volumes from a list of snapshots\. By default, the system populates the tag parameter with a `Name` key\. For the value of this key, specify a name that you want to apply to snapshots created by this process\. You can also add custom tags to this list by using the following format: `Key=Environment,Value=Test`;`Key=User,Value=TestUser1`\.

  This parameter is optional, but we recommended that you tag snapshots\. By default, the systems tags snapshots with the device ID, and `AppConsistent` \(for indicating successful, application\-consistent VSS\-enabled EBS snapshots\)\.
+ Exclude Boot Volume \(Optional\): Use this parameter to exclude boot volumes from the backup process\.

**To create VSS\-enabled EBS snapshots by using AWS Tools for Windows PowerShell**

1. Open AWS Tools for Windows PowerShell and execute the following command to specify your credentials\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.

   ```
   Set-AWSCredentials –AccessKey key_name –SecretKey key_name
   ```

1. Execute the following command to set the Region for your PowerShell session\. The example uses the us\-east\-2 Region\.

   ```
   Set-DefaultAWSRegion -Region us-east-2
   ```

1. Execute the following command to create VSS\-enabled EBS snapshots\.

   ```
   Send-SSMCommand -DocumentName AWSEC2-CreateVssSnapshot -InstanceId "$instance" -Parameter @{'ExcludeBootVolume'='False';'description'='a_description'
   ;'tags'='Key=key_name,Value=tag_value'}
   ```

If successful, the command populates the list of EBS snapshots with the new snapshots\. You can locate these snapshots in the list of EBS snapshots by searching for the tags you specified, or by searching for `AppConsistent`\. If the command execution failed, view the command output for details about why the execution failed\. If the command successfully completed, but a specific volume backup failed, you can troubleshoot the failure in the list of EBS snapshots\.

You can automate backups by creating a Maintenance Windows task that uses the AWSEC2\-CreateVssSnapshot SSM document\. For more information, see [Working with Maintenance Windows](sysman-maintenance-working.md)\.

## Creating VSS\-Enabled EBS Snapshots by Using the AWSEC2\-ManageVssIO SSM Document \(Advanced\)<a name="integration-vss-AWSEC2-ManageVssIO"></a>

You can use the following script and the pre\-defined AWSEC2\-ManageVssIO SSM document to temporarily pause I/O, create VSS\-enabled EBS snapshots, and restart I/O\. This process runs in the context of the user who executes the command\. If the user has sufficient permission to create and tag snapshots, then Systems Manager can create and tag VSS\-enabled EBS snapshots without the need for the additional IAM snapshot role on the instance\.

In contrast, the AWSEC2\-CreateVssSnapshot document requires that you assign the IAM snapshot role to each instance for which you want to create EBS snapshots\. If you don’t want to provide additional IAM permissions to your instances for policy or compliance reasons, then you can use the following script\.

**Before You Begin**  
Note the following important details about this process:
+ This process uses a PowerShell script \(CreateVssSnapshotAdvancedScript\.ps1\) to take snapshots of all volumes on the instances you specify, except root volumes\. If you need to take snapshots of root volumes, then you must use the AWSEC2\-CreateVssSnapshot SSM document\.
+ The script calls the AWSEC2\-ManageVssIO document twice\. The first time with the `Action` parameter set to `Freeze`, which pauses all I/O on the instances\. The second time, the `Action` parameter is set to `Thaw`, which forces I/O to resume\.
+ Don't attempt to use the AWSEC2\-ManageVssIO document without using the CreateVssSnapshotAdvancedScript\.ps1 script\. A limitation in VSS requires that the `Freeze` and `Thaw` actions be called no more than ten seconds apart, and manually calling these actions without the script could result in errors\.

**To create VSS\-enabled EBS snapshots by using the AWSEC2\-ManageVssIO SSM document**

1. Open AWS Tools for Windows PowerShell and execute the following command to specify your credentials\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.

   ```
   Set-AWSCredentials –AccessKey key_name –SecretKey key_name
   ```

1. Execute the following command to set the Region for your PowerShell session\. The example uses the us\-east\-2 Region\.

   ```
   Set-DefaultAWSRegion -Region us-east-2
   ```

1. Download the [CreateVssSnapshotAdvancedScript\.zip](samples/CreateVssSnapshotAdvancedScript.zip) file and extract the file contents\. 

1. Open the script in a simple text editor, edit the sample call at the bottom of the script, and then run it\.

   ```
   ```

If successful, the command populates the list of EBS snapshots with the new snapshots\. You can locate these snapshots in the list of EBS snapshots by searching for the tags you specified, or by searching for `AppConsistent`\. If the command execution failed, view the command output for details about why the execution failed\. If the command was successfully completed, but a specific volume backup failed, you can troubleshoot the failure in the list of EBS volumes\.

## Restoring Volumes from VSS\-enabled EBS snapshots<a name="integration-vss-restore"></a>

You can use the RestoreVssSnapshotSampleScript\.ps1 script to restore volumes on an instance from VSS\-enabled EBS snapshots\. This script performs the following tasks:
+ Stops an instance
+ Removes all existing drives from the instance \(except the boot volume, if it was excluded\)
+ Creates new volumes from the snapshots
+ Attaches the volumes to the instance by using the device ID tag on the snapshot
+ Restarts the instance

**Important**  
The following script detaches all volumes attached to an instance, and then creates new volumes from a snapshot\. Make sure that you have properly backed\-up the instance\. The old volumes are not deleted\. If you want, you can edit the script to delete the old volumes\.

**To restore volumes from VSS\-enabled EBS snapshots**

1. Open AWS Tools for Windows PowerShell and execute the following command to specify your credentials\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.

   ```
   Set-AWSCredentials –AccessKey key_name –SecretKey key_name
   ```

1. Execute the following command to set the Region for your PowerShell session\. The example uses the us\-east\-2 Region\.

   ```
   Set-DefaultAWSRegion -Region us-east-2
   ```

1. Download the [RestoreVssSnapshotSampleScript\.zip](samples/RestoreVssSnapshotSampleScript.zip) file and extract the file contents\. 

1. Open the script in a simple text editor, edit the sample call at the bottom of the script, and then run it\.