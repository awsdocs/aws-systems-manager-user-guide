# Setting up Systems Manager for edge devices<a name="systems-manager-setting-up-edge-devices"></a>

This section describes the setup tasks that account and system administrators perform to enable configuration and management of AWS IoT Greengrass core devices\. After you complete these tasks, users who have been granted permissions by the AWS account administrator can use AWS Systems Manager to configure and manage their organization's AWS IoT Greengrass core devices\. 

**Note**  
SSM Agent for AWS IoT Greengrass isn't supported on macOS and Windows 10\. You can't use Systems Manager capabilities to manage and configure edge devices that use these operating systems\.
Systems Manager also supports edge devices that aren't configured as AWS IoT Greengrass core devices\. To use Systems Manager to manage AWS IoT Core devices and non\-AWS edge devices, you must configure them as on\-premises machines in a hybrid environment\. For more information, see [Setting up Systems Manager for hybrid environments](systems-manager-managedinstances.md)\.
To use Session Manager and Microsoft application patching with your edge devices, you must enable the advanced\-instances tier\. For more information, see [Turning on the advanced\-instances tier](systems-manager-managedinstances-advanced.md)\.

**Before you begin**  
Verify that your edge devices meet the following requirements\.
+ Your edge devices must meet the requirements to be configured as AWS IoT Greengrass core devices\. For more information, see [Setting up AWS IoT Greengrass core devices](https://docs.aws.amazon.com/greengrass/v2/developerguide/setting-up.html) in the *AWS IoT Greengrass Version 2 Developer Guide*\.
+ Your edge devices must be compatible with AWS Systems Manager Agent \(SSM Agent\)\. For more information, see [Supported operating systems](prereqs-operating-systems.md)\.
+ Your edge devices must be able to communicate with the Systems Manager service in the cloud\. Systems Manager doesn't support disconnected edge devices\.

**About setting up edge devices**  
Setting up AWS IoT Greengrass devices for Systems Manager involves the following processes\.

**Note**  
For information about uninstalling SSM Agent from an edge device, see [Uninstall the AWS Systems Manager Agent](https://docs.aws.amazon.com/greengrass/v2/developerguide/uninstall-systems-manager-agent.html) in the *AWS IoT Greengrass Version 2 Developer Guide*\.

## Step 1: Create an IAM service role for edge devices<a name="systems-manager-setting-up-edge-devices-service-role"></a>

AWS IoT Greengrass core devices require an AWS Identity and Access Management \(IAM\) service role to communicate with AWS Systems Manager\. The role grants AWS Security Token Service \(AWS STS\) [https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) trust to the Systems Manager service\. You only need to create the service role once for each AWS account\. You will specify this role for the `RegistrationRole` parameter when you configure and deploy the SSM Agent component to your AWS IoT Greengrass devices\. If you already created this role while setting up on\-premises servers or virtual machines in a hybrid environment, you can skip this step\.

**Note**  
Users in your company or organization who will use Systems Manager on your edge devices must be granted permission in IAM to call the Systems Manager API\. 

**S3 bucket policy requirement**  
If either of the following cases are true, you must create a custom IAM permission policy for Amazon Simple Storage Service \(Amazon S3\) buckets before completing this procedure:
+ **Case 1**: You're using a VPC endpoint to privately connect your VPC to supported AWS services and VPC endpoint services powered by AWS PrivateLink\. 
+ **Case 2**: You plan to use an Amazon S3 bucket that you create as part of your Systems Manager operations, such as for storing output for Run Command commands or Session Manager sessions to an Amazon S3 bucket\. Before proceeding, follow the steps in [Create a custom S3 bucket policy for an instance profile](setup-instance-profile.md#instance-profile-custom-s3-policy)\. The information about S3 bucket policies in that topic also applies to your service role\.
**Note**  
If your devices are protected by a firewall and you plan to use Patch Manager, the firewall must allow access to the patch baseline endpoint `arn:aws:s3:::patch-baseline-snapshot-region/*`\.  
*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

------
#### [ AWS CLI ]

**To create an IAM service role for an AWS IoT Greengrass environment \(AWS CLI\)**

1. Install and configure the AWS Command Line Interface \(AWS CLI\), if you haven't already\.

   For information, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)\.

1. On your local machine, create a text file with a name such as `SSMService-Trust.json` with the following trust policy\. Make sure to save the file with the `.json` file extension\. 
**Note**  
Make a note of the name\. You will specify it when you deploy SSM Agent to your AWS IoT Greengrass core devices\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": {
           "Effect": "Allow",
           "Principal": {
               "Service": "ssm.amazonaws.com"
           },
           "Action": "sts:AssumeRole"
       }
   }
   ```

1. Open the AWS CLI, and in the directory where you created the JSON file, run the [create\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html) command to create the service role\. Replace each *example resource placeholder* with your own information\.

   **Linux & macOS**

   ```
   aws iam create-role \
       --role-name SSMServiceRole \
       --assume-role-policy-document file://SSMService-Trust.json
   ```

   **Windows**

   ```
   aws iam create-role ^
       --role-name SSMServiceRole ^
       --assume-role-policy-document file://SSMService-Trust.json
   ```

1. Run the [attach\-role\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/attach-role-policy.html) command as follows to allow the service role you just created to create a session token\. The session token gives your edge devices permission to run commands using Systems Manager\.
**Note**  
The policies you add for a service profile for edge devices are the same policies used to create an instance profile for Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. For more information about the AWS policies used in the following commands, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.

   \(Required\) Run the following command to allow an edge device to use AWS Systems Manager service core functionality\.

   **Linux & macOS**

   ```
   aws iam attach-role-policy \
       --role-name SSMServiceRole \
       --policy-arn arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
   ```

   **Windows**

   ```
   aws iam attach-role-policy ^
       --role-name SSMServiceRole ^
       --policy-arn arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
   ```

   If you created a custom S3 bucket policy for your service role, run the following command to allow AWS Systems Manager Agent \(SSM Agent\) to access the buckets you specified in the policy\. Replace *account\_ID* and *my\_bucket\_policy\_name* with your AWS account ID and your bucket name\. 

   **Linux & macOS**

   ```
   aws iam attach-role-policy \
       --role-name SSMServiceRole \
       --policy-arn arn:aws:iam::account_ID:policy/my_bucket_policy_name
   ```

   **Windows**

   ```
   aws iam attach-role-policy ^
       --role-name SSMServiceRole ^
       --policy-arn arn:aws:iam::account_id:policy/my_bucket_policy_name
   ```

   \(Optional\) Run the following command to allow SSM Agent to access AWS Directory Service on your behalf for requests to join the domain from edge devices\. The service role needs this policy only if you join your edge devices to a Microsoft AD directory\.

   **Linux & macOS**

   ```
   aws iam attach-role-policy \
       --role-name SSMServiceRole \
       --policy-arn arn:aws:iam::aws:policy/AmazonSSMDirectoryServiceAccess
   ```

   **Windows**

   ```
   aws iam attach-role-policy ^
       --role-name SSMServiceRole ^
       --policy-arn arn:aws:iam::aws:policy/AmazonSSMDirectoryServiceAccess
   ```

   \(Optional\) Run the following command to allow the CloudWatch agent to run on your edge devices\. This command makes it possible to read information on a device and write it to CloudWatch\. Your service role needs this policy only if you will use services such as Amazon EventBridge or Amazon CloudWatch Logs\.

   ```
   aws iam attach-role-policy \
       --role-name SSMServiceRole \
       --policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy
   ```

------
#### [ Tools for PowerShell ]

**To create an IAM service role for an AWS IoT Greengrass environment \(AWS Tools for Windows PowerShell\)**

1. Install and configure the AWS Tools for PowerShell \(Tools for Windows PowerShell\), if you haven't already\.

   For information, see [Installing the AWS Tools for PowerShell](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up.html)\.

1. On your local machine, create a text file with a name such as `SSMService-Trust.json` with the following trust policy\. Make sure to save the file with the `.json` file extension\.
**Note**  
Make a note of the name\. You will specify it when you deploy SSM Agent to your AWS IoT Greengrass core devices\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": {
           "Effect": "Allow",
           "Principal": {
               "Service": "ssm.amazonaws.com"
           },
           "Action": "sts:AssumeRole"
       }
   }
   ```

1. Open PowerShell in administrative mode, and in the directory where you created the JSON file, run [New\-IAMRole](https://docs.aws.amazon.com/powershell/latest/reference/items/Register-IAMRolePolicy.html) as follows to create a service role\.

   ```
   New-IAMRole `
       -RoleName SSMServiceRole `
       -AssumeRolePolicyDocument (Get-Content -raw SSMService-Trust.json)
   ```

1. Use [Register\-IAMRolePolicy](https://docs.aws.amazon.com/powershell/latest/reference/items/Register-IAMRolePolicy.html) as follows to allow the service role you created to create a session token\. The session token gives your edge devices permission to run commands using Systems Manager\.
**Note**  
The policies you add for a service role for edge devices in an AWS IoT Greengrass environment are the same policies used to create an instance profile for EC2 instances\. For more information about the AWS policies used in the following commands, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.

   \(Required\) Run the following command to allow an edge device to use AWS Systems Manager service core functionality\.

   ```
   Register-IAMRolePolicy `
       -RoleName SSMServiceRole `
       -PolicyArn arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
   ```

   If you created a custom S3 bucket policy for your service role, run the following command to allow SSM Agent to access the buckets you specified in the policy\. Replace *account\_ID* and *my\_bucket\_policy\_name* with your AWS account ID and your bucket name\. 

   ```
   Register-IAMRolePolicy `
       -RoleName SSMServiceRole `
       -PolicyArn arn:aws:iam::account_ID:policy/my_bucket_policy_name
   ```

   \(Optional\) Run the following command to allow SSM Agent to access AWS Directory Service on your behalf for requests to join the domain from edge devices\. The service role needs this policy only if you join your edge devices to a Microsoft AD directory\.

   ```
   Register-IAMRolePolicy `
       -RoleName SSMServiceRole `
       -PolicyArn arn:aws:iam::aws:policy/AmazonSSMDirectoryServiceAccess
   ```

   \(Optional\) Run the following command to allow the CloudWatch agent to run on your edge devices\. This command makes it possible to read information on a device and write it to CloudWatch\. Your service role needs this policy only if you will use services such as Amazon EventBridge or Amazon CloudWatch Logs\.

   ```
   Register-IAMRolePolicy `
       -RoleName SSMServiceRole `
       -PolicyArn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy
   ```

------

## Step 2: Set up AWS IoT Greengrass<a name="systems-manager-edge-devices-set-up-greengrass"></a>

Set up your edge devices as AWS IoT Greengrass core devices\. The setup process involves verifying supported operating systems and system requirements, as well as installing and configuring the AWS IoT Greengrass Core software on your devices\. For more information, see [Setting up AWS IoT Greengrass core devices](https://docs.aws.amazon.com/greengrass/v2/developerguide/setting-up.html) in the *AWS IoT Greengrass Version 2 Developer Guide*\.

## Step 3: Update the AWS IoT Greengrass token exchange role and install SSM Agent on your edge devices<a name="systems-manager-edge-devices-install-SSM-agent"></a>

The final step for setting up and configuring your AWS IoT Greengrass core devices for Systems Manager requires you to update the AWS IoT Greengrass AWS Identity and Access Management \(IAM\) device service role, also called the *token exchange role*, and deploy AWS Systems Manager Agent \(SSM Agent\) to your AWS IoT Greengrass devices\. Both processes are described in detail in the *AWS IoT Greengrass Version 2 Developer Guide*\. For more information, see [Install the AWS Systems Manager Agent](https://docs.aws.amazon.com/greengrass/v2/developerguide/install-systems-manager-agent.html)\.

After you deploy SSM Agent to your devices, AWS IoT Greengrass automatically registers your devices with Systems Manager\. No additional registration is necessary\. You can begin using Systems Manager capabilities to access, manage, and configure your AWS IoT Greengrass devices\.

**Note**  
Your edge devices must be able to communicate with the Systems Manager service in the cloud\. Systems Manager doesn't support disconnected edge devices\.