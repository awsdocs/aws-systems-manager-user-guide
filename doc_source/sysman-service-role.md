# Create an IAM Service Role for a Hybrid Environment<a name="sysman-service-role"></a>

Servers and VMs in a hybrid environment require an IAM role to communicate with the Systems Manager SSM service\. The role grants AssumeRole trust to the SSM service\. 

**Note**  
You only need to create the service role once for each AWS account\.

**To create an IAM service role using AWS Tools for Windows PowerShell**

1. Create a text file \(in this example it is named `SSMService-Trust.json`\) with the following trust policy\. Save the file with the `.json` file extension\.

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

1. Use [New\-IAMRole](http://docs.aws.amazon.com/powershell/latest/reference/items/New-IAMRole.html) as follows to create a service role\. This example creates a role named SSMServiceRole\.

   ```
   New-IAMRole -RoleName SSMServiceRole -AssumeRolePolicyDocument (Get-Content -raw SSMService-Trust.json)
   ```

1. Use [Register\-IAMRolePolicy](http://docs.aws.amazon.com/powershell/latest/reference/items/Register-IAMRolePolicy.html) as follows to enable the SSMServiceRole to create a session token\. The session token gives your managed instance permission to run commands using Systems Manager\.

   ```
   Register-IAMRolePolicy -RoleName SSMServiceRole -PolicyArn arn:aws:iam::aws:policy/service-role/AmazonEC2RoleforSSM
   ```

**To create an IAM service role using the AWS CLI**

1. Create a text file \(in this example it is named `SSMService-Trust.json`\) with the following trust policy\. Save the file with the `.json` file extension\.

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

1. Use the [create\-role](http://docs.aws.amazon.com/cli/latest/reference/iam/create-role.html) command to create the service role\. This example creates a role named SSMServiceRole\.

   ```
   aws iam create-role --role-name SSMServiceRole --assume-role-policy-document file://SSMService-Trust.json 
   ```

1. Use [attach\-role\-policy](http://docs.aws.amazon.com/cli/latest/reference/iam/attach-role-policy.html) as follows to enable the SSMServiceRole to create a session token\. The session token gives your managed instance permission to run commands using Systems Manager\.

   ```
   aws iam attach-role-policy --role-name SSMServiceRole --policy-arn arn:aws:iam::aws:policy/service-role/AmazonEC2RoleforSSM 
   ```

**Note**  
Users in your company or organization who will use Systems Manager on your hybrid machines must be granted permission in IAM to call the SSM API\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\.