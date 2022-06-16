# Step 2: Create an IAM service role for a hybrid environment<a name="sysman-service-role"></a>

Servers and virtual machines \(VMs\) in a hybrid environment require an AWS Identity and Access Management \(IAM\) role to communicate with the AWS Systems Manager service\. The role grants AWS Security Token Service \(AWS STS\) [https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) trust to the Systems Manager service\. You only need to create a service role for a hybrid environment once for each AWS account\. However, you might choose to create multiple service roles for different hybrid activations if machines in your hybrid environment require different permissions\.

The following procedures describe how to create the required service role using the Systems Manager console or your preferred command line tool\.

## Create an IAM service role \(console\)<a name="sysman-service-role-console"></a>

Use the following procedure to create a service role for hybrid activation\. Please note that this procedure uses the `AmazonSSMManagedInstanceCore` policy for Systems Manager core functionality\. Depending on your use case, you might need to add additional policies to your service role for your on\-premises machines to be able to access other capabilities or AWS services\. For example, without access to the required AWS managed Amazon Simple Storage Service \(Amazon S3\) buckets, Patch Manager patching operations fail\.

**To create a service role \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. For **Select trusted entity**, make the following choices:

   1. For **Trusted entity type**, choose **AWS service**

   1. For **Use cases for other AWS services**, choose **Systems Manager**

   1. Choose **Systems Manager**, as shown in the following image\.  
![\[Screenshot illustrating the Systems Manager option selected as a use case.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/iam_use_cases_for_MWs.png)

1. Choose **Next**\. 

1. On the **Add permissions** page, do the following: 
   + Use the **Search** field to locate the **AmazonSSMManagedInstanceCore** policy\. Select the check box next to its name\.   
![\[Choosing the EC2 service in the IAM console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/setup-instance-profile-2.png)
   + The console retains your selection even if you search for other policies\.
   + If you created a custom S3 bucket policy in the procedure [Task 1: \(Optional\) Create a custom policy for S3 bucket access](setup-instance-profile.md#instance-profile-custom-s3-policy), search for it and select the check box next to its name\.
   + If you plan to join instances to an Active Directory managed by AWS Directory Service, search for **AmazonSSMDirectoryServiceAccess** and select the check box next to its name\.
   + If you plan to use EventBridge or CloudWatch Logs to manage or monitor your instance, search for **CloudWatchAgentServerPolicy** and select the check box next to its name\.

1. Choose **Next**\.

1. For **Role name**, enter a name for your new instance profile, such as **SSMInstanceProfile**\.
**Note**  
Make a note of the role name\. You will choose this role when you create new instances that you want to manage by using Systems Manager\.

1. \(Optional\) For **Description**, update the description for this instance profile\.

1. \(Optional\) For **Tags**, add one or more tag\-key value pairs to organize, track, or control access for this role, and then choose **Create role**\. The system returns you to the **Roles** page\.

1. Choose **Create role**\. The system returns you to the **Roles** page\.

## Create an IAM service role \(command line\)<a name="sysman-service-role-cli"></a>

Use the following procedure to create a service role for hybrid activation\. Please note that this procedure uses the **AmazonSSMManagedInstanceCore** policy Systems Manager core functionality\. Depending on your use case, you might need to add additional policies to your service role for your on\-premises machines to be able to access other capabilities or AWS services\.

**S3 bucket policy requirement**  
If either of the following cases are true, you must create a custom IAM permission policy for Amazon Simple Storage Service \(Amazon S3\) buckets before completing this procedure:
+ **Case 1**: You're using a VPC endpoint to privately connect your VPC to supported AWS services and VPC endpoint services powered by AWS PrivateLink\. 
+ **Case 2**: You plan to use an Amazon S3 bucket that you create as part of your Systems Manager operations, such as for storing output for Run Command commands or Session Manager sessions to an Amazon S3 bucket\. Before proceeding, follow the steps in [Create a custom S3 bucket policy for an instance profile](setup-instance-profile.md#instance-profile-custom-s3-policy)\. The information about S3 bucket policies in that topic also applies to your service role\.

------
#### [ AWS CLI ]

**To create an IAM service role for a hybrid environment \(AWS CLI\)**

1. Install and configure the AWS Command Line Interface \(AWS CLI\), if you haven't already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. On your local machine, create a text file with a name such as `SSMService-Trust.json` with the following trust policy\. Make sure to save the file with the `.json` file extension\. Be sure to specify your AWS account and the AWS Region in the ARN where you created your hybrid activation\.

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Sid":"",
            "Effect":"Allow",
            "Principal":{
               "Service":"ssm.amazonaws.com"
            },
            "Action":"sts:AssumeRole",
            "Condition":{
               "StringEquals":{
                  "aws:SourceAccount":"123456789012"
               },
               "ArnEquals":{
                  "aws:SourceArn":"arn:aws:ssm:us-east-2:123456789012:*"
               }
            }
         }
      ]
   }
   ```

1. Open the AWS CLI, and in the directory where you created the JSON file, run the [create\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html) command to create the service role\. This example creates a role named `SSMServiceRole`\. You can choose another name if you prefer\.

------
#### [ Linux & macOS ]

   ```
   aws iam create-role \
       --role-name SSMServiceRole \
       --assume-role-policy-document file://SSMService-Trust.json
   ```

------
#### [ Windows ]

   ```
   aws iam create-role ^
       --role-name SSMServiceRole ^
       --assume-role-policy-document file://SSMService-Trust.json
   ```

------

1. Run the [attach\-role\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/attach-role-policy.html) command as follows to allow the service role you just created to create a session token\. The session token gives your managed instance permission to run commands using Systems Manager\.
**Note**  
The policies you add for a service profile for managed instances in a hybrid environment are the same policies used to create an instance profile for Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. For more information about the AWS policies used in the following commands, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.

   \(Required\) Run the following command to allow a managed instance to use AWS Systems Manager service core functionality\.

------
#### [ Linux & macOS ]

   ```
   aws iam attach-role-policy \
       --role-name SSMServiceRole \
       --policy-arn arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
   ```

------
#### [ Windows ]

   ```
   aws iam attach-role-policy ^
       --role-name SSMServiceRole ^
       --policy-arn arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
   ```

------

   If you created a custom S3 bucket policy for your service role, run the following command to allow AWS Systems Manager Agent \(SSM Agent\) to access the buckets you specified in the policy\. Replace *account\-id* and *my\-bucket\-policy\-name* with your AWS account ID and your bucket name\. 

------
#### [ Linux & macOS ]

   ```
   aws iam attach-role-policy \
       --role-name SSMServiceRole \
       --policy-arn arn:aws:iam::account-id:policy/my-bucket-policy-name
   ```

------
#### [ Windows ]

   ```
   aws iam attach-role-policy ^
       --role-name SSMServiceRole ^
       --policy-arn arn:aws:iam::account-id:policy/my-bucket-policy-name
   ```

------

   \(Optional\) Run the following command to allow SSM Agent to access AWS Directory Service on your behalf for requests to join the domain by the managed instance\. Your instance profile needs this policy only if you join your instances to a Microsoft AD directory\.

------
#### [ Linux & macOS ]

   ```
   aws iam attach-role-policy \
       --role-name SSMServiceRole \
       --policy-arn arn:aws:iam::aws:policy/AmazonSSMDirectoryServiceAccess
   ```

------
#### [ Windows ]

   ```
   aws iam attach-role-policy ^
       --role-name SSMServiceRole ^
       --policy-arn arn:aws:iam::aws:policy/AmazonSSMDirectoryServiceAccess
   ```

------

   \(Optional\) Run the following command to allow the CloudWatch agent to run on your managed instances\. This command makes it possible to read information on an instance and write it to CloudWatch\. Your service profile needs this policy only if you will use services such as Amazon EventBridge or Amazon CloudWatch Logs\.

   ```
   aws iam attach-role-policy \
       --role-name SSMServiceRole \
       --policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy
   ```

------
#### [ Tools for PowerShell ]

**To create an IAM service role for a hybrid environment \(AWS Tools for Windows PowerShell\)**

1. Install and configure the AWS Tools for PowerShell, if you haven't already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. On your local machine, create a text file with a name such as `SSMService-Trust.json` with the following trust policy\. Make sure to save the file with the `.json` file extension\. Be sure to specify your AWS account and the AWS Region in the ARN where you created your hybrid activation\.

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Sid":"",
            "Effect":"Allow",
            "Principal":{
               "Service":"ssm.amazonaws.com"
            },
            "Action":"sts:AssumeRole",
            "Condition":{
               "StringEquals":{
                  "aws:SourceAccount":"123456789012"
               },
               "ArnEquals":{
                  "aws:SourceArn":"arn:aws:ssm:region:123456789012:*"
               }
            }
         }
      ]
   }
   ```

1. Open PowerShell in administrative mode, and in the directory where you created the JSON file, run [New\-IAMRole](https://docs.aws.amazon.com/powershell/latest/reference/items/Register-IAMRolePolicy.html) as follows to create a service role\. This example creates a role named `SSMServiceRole`\. You can choose another name if you prefer\.

   ```
   New-IAMRole `
       -RoleName SSMServiceRole `
       -AssumeRolePolicyDocument (Get-Content -raw SSMService-Trust.json)
   ```

1. Use [Register\-IAMRolePolicy](https://docs.aws.amazon.com/powershell/latest/reference/items/Register-IAMRolePolicy.html) as follows to allow the service role you created to create a session token\. The session token gives your managed instance permission to run commands using Systems Manager\.
**Note**  
The policies you add for a service profile for managed instances in a hybrid environment are the same policies used to create an instance profile for EC2 instances\. For more information about the AWS policies used in the following commands, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.

   \(Required\) Run the following command to allow a managed instance to use AWS Systems Manager service core functionality\.

   ```
   Register-IAMRolePolicy `
       -RoleName SSMServiceRole `
       -PolicyArn arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
   ```

   If you created a custom S3 bucket policy for your service role, run the following command to allow SSM Agent to access the buckets you specified in the policy\. Replace *account\-id* and *my\-bucket\-policy\-name* with your AWS account ID and your bucket name\. 

   ```
   Register-IAMRolePolicy `
       -RoleName SSMServiceRole `
       -PolicyArn arn:aws:iam::account-id:policy/my-bucket-policy-name
   ```

   \(Optional\) Run the following command to allow SSM Agent to access AWS Directory Service on your behalf for requests to join the domain by the managed instance\. Your instance profile needs this policy only if you join your instances to a Microsoft AD directory\.

   ```
   Register-IAMRolePolicy `
       -RoleName SSMServiceRole `
       -PolicyArn arn:aws:iam::aws:policy/AmazonSSMDirectoryServiceAccess
   ```

   \(Optional\) Run the following command to allow the CloudWatch agent to run on your managed instances\. This command makes it possible to read information on an instance and write it to CloudWatch\. Your service profile needs this policy only if you will use services such as Amazon EventBridge or Amazon CloudWatch Logs\.

   ```
   Register-IAMRolePolicy `
       -RoleName SSMServiceRole `
       -PolicyArn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy
   ```

------

Continue to [Step 3: Create a managed\-instance activation for a hybrid environment](sysman-managed-instance-activation.md)\.