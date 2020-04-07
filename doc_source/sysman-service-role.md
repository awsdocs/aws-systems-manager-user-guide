# Step 2: Create an IAM Service Role for a Hybrid Environment<a name="sysman-service-role"></a>

Servers and virtual machines \(VMs\) in a hybrid environment require an IAM role to communicate with the Systems Manager service\. The role grants `AssumeRole` trust to the Systems Manager service\. You only need to create the service role for a hybrid environment once for each AWS account\.

**Note**  
Users in your company or organization who will use Systems Manager on your hybrid machines must be granted permission in IAM to call the SSM API\. For more information, see [ Create Non\-Admin IAM Users and Groups for Systems Manager](setup-create-iam-user.md)\.

**S3 bucket policy requirement**  
If either of the following cases are true, you must create a custom IAM permission policy for Amazon S3 buckets before completing this procedure:
+ **Case 1**: You are using a VPC endpoint to privately connect your VPC to supported AWS services and VPC endpoint services powered by PrivateLink\. 
+ **Case 2**: You plan to use an Amazon S3 bucket that you create as part of your Systems Manager operations, such as for storing output for Run Command commands or Session Manager sessions to an S3 bucket\. Before proceeding, follow the steps in [Create a Custom S3 Bucket Policy for an Instance Profile](setup-instance-profile.md#instance-profile-custom-s3-policy)\. The information about S3 bucket policies in that topic also applies to your service role\.

------
#### [ AWS CLI ]

**To create an IAM service role for a hybrid environment \(AWS CLI\)**

1. Create a text file with a name such as `SSMService-Trust.json` with the following trust policy\. Make sure to save the file with the `.json` file extension\.

   ```
   {
               "Version": "2012-10-17",
               "Statement": {
                   "Effect": "Allow",
                   "Principal": {"Service": "ssm.amazonaws.com"},
                   "Action": "sts:AssumeRole"
               }
               }
   ```

1. Use the [create\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html) command to create the service role\. This example creates a role named `SSMServiceRole`\. You can choose another name if you prefer\.

   ```
   aws iam create-role \ 
       --role-name SSMServiceRole \
       --assume-role-policy-document file://SSMService-Trust.json
   ```

1. Use [attach\-role\-policy](https://docs.aws.amazon.com/cli/latest/reference/iam/attach-role-policy.html) as follows to enable the service role you just created to create a session token\. The session token gives your managed instance permission to run commands using Systems Manager\.
**Note**  
The policies you add for a service profile for managed instances in a hybrid environment are the same policies used to create an instance profile for EC2 instances\. For more information about the AWS policies used in the following commands, see [Create an IAM Instance Profile for Systems Manager](setup-instance-profile.md)\.

   \(Required\) Run the following command to enable a managed instance to use AWS Systems Manager service core functionality\.

   ```
   aws iam attach-role-policy \
       --role-name SSMServiceRole \
       --policy-arn arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
   ```

   If you created a custom S3 bucket policy for your service role, run the following command to enable SSM Agent to access the buckets you specified in the policy\. Replace *account\-id* and *my\-bucket\-policy\-name* with your AWS account ID and your bucket name\. 

   ```
   aws iam attach-role-policy \
       --role-name SSMServiceRole \
       --policy-arn arn:aws:iam::account-id:policy/my-bucket-policy-name
   ```

   \(Optional\) Run the following command to allow SSM Agent to access AWS Directory Service on your behalf for requests to join the domain by the managed instance\. Your instance profile needs this policy only if you join your instances to a Microsoft AD directory\.

   ```
   aws iam attach-role-policy \
       --role-name SSMServiceRole \
       --policy-arn arn:aws:iam::aws:policy/AmazonSSMDirectoryServiceAccess
   ```

   \(Optional\) Run the following command to allow the CloudWatch agent to run on your managed instances\. This command makes it possible to read information on an instance and write it to CloudWatch\. Your service profile needs this policy only if you will use CloudWatch features, such as Amazon CloudWatch Events or Amazon CloudWatch Logs\.

   ```
   aws iam attach-role-policy \
       --role-name SSMServiceRole \
       --policy-arn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy
   ```

------
#### [ Tools for PowerShell ]

**To create an IAM service role for a hybrid environment \(AWS Tools for Windows PowerShell\)**

1. Create a text file with a name such as `SSMService-Trust.json` with the following trust policy\. Make sure to save the file with the `.json` file extension\.

   ```
   {
               "Version": "2012-10-17",
               "Statement": {
                   "Effect": "Allow",
                   "Principal": {"Service": "ssm.amazonaws.com"},
                   "Action": "sts:AssumeRole"
               }
               }
   ```

1. Use [New\-IAMRole](https://docs.aws.amazon.com/powershell/latest/reference/items/New-IAMRole.html) as follows to create a service role\. This example creates a role named `SSMServiceRole`\. You can choose another name if you prefer\.

   ```
   New-IAMRole `
       -RoleName SSMServiceRole `
       -AssumeRolePolicyDocument (Get-Content -raw SSMService-Trust.json)
   ```

1. Use [Register\-IAMRolePolicy](https://docs.aws.amazon.com/powershell/latest/reference/items/Register-IAMRolePolicy.html) as follows to enable the service role you created to create a session token\. The session token gives your managed instance permission to run commands using Systems Manager\.
**Note**  
The policies you add for a service profile for managed instances in a hybrid environment are the same policies used to create an instance profile for EC2 instances\. For more information about the AWS policies used in the following commands, see [Create an IAM Instance Profile for Systems Manager](setup-instance-profile.md)\.

   \(Required\) Run the following command to enable a managed instance to use AWS Systems Manager service core functionality\.

   ```
   Register-IAMRolePolicy `
       -RoleName SSMServiceRole `
       -PolicyArn arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore
   ```

   If you created a custom S3 bucket policy for your service role, run the following command to enable SSM Agent to access the buckets you specified in the policy\. Replace *account\-id* and *my\-bucket\-policy\-name* with your AWS account ID and your bucket name\. 

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

   \(Optional\) Run the following command to allow the CloudWatch agent to run on your managed instances\. This command makes it possible to read information on an instance and write it to CloudWatch\. Your service profile needs this policy only if you will use CloudWatch features, such as Amazon CloudWatch Events or Amazon CloudWatch Logs\.

   ```
   Register-IAMRolePolicy `
       -RoleName SSMServiceRole `
       -PolicyArn arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy
   ```

------

**Note**  
Users in your company or organization who are to use Systems Manager on your hybrid machines must be granted permission in IAM to call the SSM API\. For more information, see [Create Users and Assign Permissions](setup-create-users-nonadmin-users.md)\.

Continue to [Step 3: Install a TLS certificate on On\-Premises Servers and VMs](hybrid-tls-certificate.md)\.