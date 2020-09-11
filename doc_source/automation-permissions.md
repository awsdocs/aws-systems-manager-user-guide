# Method 2: Use IAM to configure roles for Automation<a name="automation-permissions"></a>

If you need to create a service role for Systems Manager Automation, complete the following tasks\. For more information on when a service role is required for Automation, see [Getting started with Automation](automation-setup.md)\.

**Topics**
+ [Task 1: Create a service role for Automation](#automation-role)
+ [Task 2: Attach the iam:PassRole policy to your Automation role](#automation-passpolicy)
+ [Task 3: Configure user access to Automation](#automation-passrole)

## Task 1: Create a service role for Automation<a name="automation-role"></a>

Use the following procedure to create a service role \(or *assume role*\) for Systems Manager Automation\.

**Note**  
You can also use this role in Automation documents, such as the `AWS-CreateManagedLinuxInstance` document\. Using this role or the ARNs in Automation documents enables Automation to perform actions in your environment, such as launch new instances and perform actions on your behalf\.

**To create an IAM role and allow Automation to assume it**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. Under **Select type of trusted entity**, choose **AWS service**\.

1. In the **Choose a use case** section, choose **Systems Manager**, and then choose **Next: Permissions**\.

1. On the **Attached permissions policy** page, search for the **AmazonSSMAutomationRole** policy, choose it, and then choose **Next: Review**\. 

1. On the **Review** page, type a name in the **Role name** box, and then type a description\.

1. Choose **Create role**\. The system returns you to the **Roles** page\.

1. On the **Roles** page, choose the role you just created to open the **Summary** page\. Note the **Role Name** and **Role ARN**\. You will specify the role ARN when you attach the **iam:PassRole** policy to your IAM account in the next procedure\. You can also specify the role name and the ARN in Automation documents\.

**Note**  
The AmazonSSMAutomationRole policy assigns the Automation role permission to a subset of AWS Lambda functions within your account\. These functions begin with "Automation"\. If you plan to use Automation with Lambda functions, the Lambda ARN must use the following format:  
`"arn:aws:lambda:*:*:function:Automation*"`  
If you have existing Lambda functions whose ARNs do not use this format, then you must also attach an additional Lambda policy to your automation role, such as the **AWSLambdaRole** policy\. The additional policy or role must provide broader access to Lambda functions within the AWS account\.

### \(Optional\) add an Automation inline policy to invoke other AWS services<a name="automation-role-add-inline-policy"></a>

If you run an automation that invokes other AWS services by using an IAM service role, the service role must be configured with permission to invoke those services\. This requirement applies to all AWS Automation documents \(AWS\-\* documents\) such as the `AWS-ConfigureS3BucketLogging`, `AWS-CreateDynamoDBBackup`, and `AWS-RestartEC2Instance` documents, to name a few\. This requirement also applies to any custom Automation documents you create that invoke other AWS services by using actions that call other services\. For example, if you use the `aws:executeAwsApi`, `aws:CreateStack`, or `aws:copyImage` actions, to name a few, then you must configure the service role with permission to invoke those services\. You can enable permissions to other AWS services by adding an IAM inline policy to the role\. 

**To embed an inline policy for a service role \(IAM console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. In the list, choose the name of the role that you want to edit\.

1. Choose the **Permissions** tab\.

1. Choose **Add inline policy**\.

1. Choose the **JSON** tab\.

1. Enter a JSON policy document for the AWS services you want to invoke\. Here are two example JSON policy documents\.

   **Amazon S3 PutObject and GetObject Example**

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "s3:PutObject",
                   "s3:GetObject"
               ],
               "Resource": "arn:aws:s3:::DOC-EXAMPLE-BUCKET/*"
           }
       ]
   }
   ```

   **Amazon EC2 CreateSnapshot and DescribeSnapShots Example**

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Effect":"Allow",
            "Action":"ec2:CreateSnapshot",
            "Resource":"*"
         },
         {
            "Effect":"Allow",
            "Action":"ec2:DescribeSnapshots",
            "Resource":"*"
         }
      ]
   }
   ```

   For details about the IAM policy language, see [IAM JSON Policy Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

1. When you are finished, choose **Review policy**\. The [Policy Validator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_policy-validator.html) reports any syntax errors\.

1. On the **Review policy** page, enter a **Name** for the policy that you are creating\. Review the policy **Summary** to see the permissions that are granted by your policy\. Then choose **Create policy** to save your work\.

1. After you create an inline policy, it is automatically embedded in your role\.

## Task 2: Attach the iam:PassRole policy to your Automation role<a name="automation-passpolicy"></a>

Use the following procedure to attach the `iam:PassRole` policy to your Automation service role\. This enables the Automation service to pass the role to other services or Systems Manager capabilities when running Automation workflows\.

**To attach the iam:PassRole policy to your Automation role**

1. In the **Summary** page for the role you just created, choose the **Permissions** tab\.

1. Choose **Add inline policy**\.

1. On the **Create policy** page, choose the **Visual editor** tab\.

1. Choose **Service**, and then choose **IAM**\.

1. Choose **Select actions**\.

1. In the **Filter actions** text box, type **PassRole**, and then choose the **PassRole** option\.

1. Choose **Resources**\. Verify that **Specific** is selected, and then choose **Add ARN**\.

1. In the **Specify ARN for role** field, paste the Automation role ARN that you copied at the end of Task 1\. The system populates the **Account** and **Role name with path** fields\.
**Note**  
If you want the Automation service role to attach an IAM instance profile role to an EC2 instance, then you must add the ARN of the IAM instance profile role\. This allows the Automation service role to pass the IAM instance profile role to the target EC2 instance\.

1. Choose **Add**\.

1. Choose **Review policy**\.

1. On the **Review Policy** page, type a name and then choose **Create Policy**\.

## Task 3: Configure user access to Automation<a name="automation-passrole"></a>

If your AWS Identity and Access Management \(IAM\) user account, group, or role is assigned administrator permissions, then you have access to Systems Manager Automation\. If you don't have administrator permissions, then an administrator must give you permission by assigning the `AmazonSSMFullAccess` managed policy, or a policy that provides comparable permissions, to your IAM account, group, or role\.

Use the following procedure to configure a user account to use Automation\. The user account you choose will have permission to configure and run Automation\. If you need to create a new user account, see [Creating an IAM User in Your AWS Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html) in the *IAM User Guide*\.

**To configure user access and attach the iam:PassRole policy to a user account**

1. In the IAM navigation pane, choose **Users**, and then choose the user account you want to configure\.

1. On the **Permissions** tab, in the policies list, verify that either the **AmazonSSMFullAccess** policy is listed or there is a comparable policy that gives the account permissions to access Systems Manager\.

1. Choose **Add inline policy**\.

1. On the **Create policy** page, choose **Visual Editor**, and then choose **Choose a service**\.

1. From **AWS Services**, choose **AWS Identity and Access Management**\.

1. For **Actions**, enter **PassRole** in the **Filter actions** prompt, and choose **PassRole**\.

1. In the **Resources** section, choose **Add ARN**, paste the ARN for the Automation service role you copied at the end of Task 1, and then choose **Add**\.

1. Choose **Review policy**\.

1. On the **Review Policy** page, provide a **Name** for the policy and then choose **Create policy**\.

You have finished configuring the required roles for Automation\. You can now use the Automation service role ARN in your Automation documents\.