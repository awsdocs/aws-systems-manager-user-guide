# Configuring roles and permissions for Change Manager<a name="change-manager-permissions"></a>

By default, Change Manager doesn't have permission to perform actions on your resources\. You must grant access by using an AWS Identity and Access Management \(IAM\) service role, or *assume role*\. This role enables Change Manager to securely run the runbook workflows specified in an approved change request on your behalf\. The role grants AWS Security Token Service \(AWS STS\) [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) trust to Change Manager\.

By providing these permissions to a role to act on behalf of users in an organization, users don't need to be granted that array of permissions themselves\. The actions allowed by the permissions are limited to approved operations only\.

When users in your account or organization create a change request, they can select this assume role to perform the change operations\.

You can create a new assume role for Change Manager or update an existing role with the needed permissions\.

If you need to create a service role for Change Manager, complete the following tasks\. 

**Topics**
+ [Task 1: Creating an assume role policy for Change Manager](#change-manager-role-policy)
+ [Task 2: Creating an assume role for Change Manager](#change-manager-role)
+ [Task 3: Attaching the `iam:PassRole` policy to other roles](#change-manager-passpolicy)
+ [Task 4: Adding inline policies to an assume role to invoke other AWS services](#change-manager-role-add-inline-policy)
+ [Task 5: Configuring user access to Change Manager](#change-manager-passrole)

## Task 1: Creating an assume role policy for Change Manager<a name="change-manager-role-policy"></a>

Use the following procedure to create the policy that you will attach to your Change Manager assume role\.

**To create an assume role policy for Change Manager**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create Policy**\.

1. On the **Create policy** page, choose the **JSON** tab and replace the default content with the following, which you will modify for your own Change Manager operations in following steps\.
**Note**  
If you're creating a policy to use with a single AWS account, and not an organization with multiple accounts and AWS Regions, you can omit the first statement block\. The `iam:PassRole` permission isn't required in the case of a single account using Change Manager\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "iam:PassRole",
               "Resource": "arn:aws:iam::delegated-admin-account-id:role/AWS-SystemsManager-job-functionAdministrationRole",
               "Condition": {
                   "StringEquals": {
                       "iam:PassedToService": "ssm.amazonaws.com"
                   }
               }
           },
           {
               "Effect": "Allow",
               "Action": [
                   "ssm:DescribeDocument",
                   "ssm:GetDocument",
                   "ssm:StartChangeRequestExecution"
               ],
               "Resource": [
                   "arn:aws:ssm:region:account-id:automation-definition/template-name:$DEFAULT",
                   "arn:aws:ssm:region::document/template-name"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "ssm:ListOpsItemEvents",
                   "ssm:GetOpsItem",
                   "ssm:ListDocuments",
                   "ssm:DescribeOpsItems"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

1. For the `iam:PassRole` action, update the `Resource` value to include the ARNs of all job functions defined for your organization that you want to grant permissions to initiate runbook workflows\.

1. Replace the *region*, *account\-id*, *template\-name*, *delegated\-admin\-account\-id*, and *job\-function* placeholders with values for your Change Manager operations\.

1. For the second `Resource` statement, modify the list to include all change templates that you want to grant permissions for\. Alternatively, specify `"Resource": "*"` to grant permissions for all change templates in your organization\.

1. Choose **Next: Tags**\.

1. \(Optional\) Add one or more tag\-key value pairs to organize, track, or control access for this policy\. 

1. Choose **Next: Review**\.

1. On the **Review policy** page, enter a name in the **Name** box, such as **MyChangeManagerAssumeRole**, and then enter an optional description\.

1. Choose **Create policy**, and continue to [Task 2: Creating an assume role for Change Manager](#change-manager-role)\.

## Task 2: Creating an assume role for Change Manager<a name="change-manager-role"></a>

Use the following procedure to create a Change Manager assume role, a type of service role, for Change Manager\.

**To create an assume role for Change Manager**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. For **Select trusted entity**, make the following choices:

   1. For **Trusted entity type**, choose **AWS service**

   1. For **Use cases for other AWS services**, choose **Systems Manager**

   1. Choose **Systems Manager**, as shown in the following image\.  
![\[Screenshot illustrating the Systems Manager option selected as a use case.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/iam_use_cases_for_MWs.png)

1. Choose **Next**\.

1. On the **Attached permissions policy** page, search for the assume role policy you created in [Task 1: Creating an assume role policy for Change Manager](#change-manager-role-policy), such as **MyChangeManagerAssumeRole**\. 

1. Select the check box next to the assume role policy name, and then choose **Next: Tags**\.

1. For **Role name**, enter a name for your new instance profile, such as **MyChangeManagerAssumeRole**\.

1. \(Optional\) For **Description**, update the description for this instance role\.

1. \(Optional\) Add one or more tag\-key value pairs to organize, track, or control access for this role\. 

1. Choose **Next: Review**\.

1. \(Optional\) For **Tags**, add one or more tag\-key value pairs to organize, track, or control access for this role, and then choose **Create role**\. The system returns you to the **Roles** page\.

1. Choose **Create role**\. The system returns you to the **Roles** page\.

1. On the **Roles** page, choose the role you just created to open the **Summary** page\. 

## Task 3: Attaching the `iam:PassRole` policy to other roles<a name="change-manager-passpolicy"></a>

Use the following procedure to attach the `iam:PassRole` policy to an IAM instance profile or IAM service role\. \(The Systems Manager service uses IAM instance profiles to communicate with EC2 instances\. For on\-premises instances or virtual machines \(VMs\), or *hybrid instances*, an IAM service role is used instead\.\)

By attaching the `iam:PassRole` policy, the Change Manager service can pass assume role permissions to other services or Systems Manager capabilities when running runbook workflows\.

**To attach the `iam:PassRole` policy to an IAM instance profile or service role for hybrid instances**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. Search for the Change Manager assume role you created, such as **MyChangeManagerAssumeRole**, and choose its name\.

1. In the **Summary** page for the assume role, choose the **Permissions** tab\.

1. Choose **Add permissions, Create inline policy**\.

1. On the **Create policy** page, choose the **Visual editor** tab\.

1. Choose **Service**, and then choose **IAM**\.

1. In the **Filter actions** text box, enter **PassRole**, and then choose the **PassRole** option\.

1. Expand **Resources**\. Verify that **Specific** is selected, and then choose **Add ARN**\.

1. In the **Specify ARN for role** field, enter the ARN of the IAM instance profile role or IAM service role to which you want to pass assume role permissions\. The system populates the **Account** and **Role name with path** fields\. 

1. Choose **Add**\.

1. Choose **Review policy**\.

1. For **Name**, enter a name to identify this policy, and then choose **Create policy**\.

**Related content**
+ [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)
+ [Create an IAM service role for a hybrid environment](sysman-service-role.md)

## Task 4: Adding inline policies to an assume role to invoke other AWS services<a name="change-manager-role-add-inline-policy"></a>

When a change request invokes other AWS services by using the Change Manager assume role, the assume role must be configured with permission to invoke those services\. This requirement applies to all AWS Automation runbooks \(AWS\-\* runbooks\) that might be used in a change request, such as the `AWS-ConfigureS3BucketLogging`, `AWS-CreateDynamoDBBackup`, and `AWS-RestartEC2Instance` runbooks\. This requirement also applies to any custom runbooks you create that invoke other AWS services by using actions that call other services\. For example, if you use the `aws:executeAwsApi`, `aws:CreateStack`, or `aws:copyImage` actions, then you must configure the service role with permission to invoke those services\. You can enable permissions to other AWS services by adding an IAM inline policy to the role\. 

**To add an inline policy to an assume role to invoke other AWS services \(IAM console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. In the list, choose the name of the assume role that you want to update, such as `MyChangeManagerAssumeRole`\.

1. Choose the **Permissions** tab\.

1. Choose **Add permissions, Create inline policy**\.

1. Choose the **JSON** tab\.

1. Enter a JSON policy document for the AWS services you want to invoke\. Here are two example JSON policy documents\.

   **Amazon S3 `PutObject` and `GetObject` example**

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

   **Amazon EC2 `CreateSnapshot` and `DescribeSnapShots` example**

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

    For details about the IAM policy language, see [IAM JSON policy reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

1. When you're finished, choose **Review policy**\. The [Policy Validator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_policy-validator.html) reports any syntax errors\.

1. For **Name**, enter a name to identify the policy that you're creating\. Review the policy **Summary** to see the permissions that are granted by your policy\. Then choose **Create policy** to save your work\.

1. After you create an inline policy, it's automatically embedded in your role\.

## Task 5: Configuring user access to Change Manager<a name="change-manager-passrole"></a>

If your IAM user account, group, or role is assigned administrator permissions, then you have access to Change Manager\. If you don't have administrator permissions, then an administrator must assign the `AmazonSSMFullAccess` managed policy, or a policy that provides comparable permissions, to your IAM account, group, or role\.

Use the following procedure to configure a user account to use Change Manager\. The user account you choose will have permission to configure and run Change Manager\. If you need to create a new user account, see [Creating an IAM user in your AWS account](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html) in the *IAM User Guide*\.

**To configure user access and attach the `iam:PassRole` policy to a user account**

1. In the IAM navigation pane, choose **Users**, and then choose the user account you want to configure\.

1. On the **Permissions** tab, in the policies list, verify that either the **AmazonSSMFullAccess** policy or a comparable policy that gives the account permissions to access Systems Manager is listed\.

1. Choose **Add inline policy**\.

1. On the **Create policy** page, on the **Visual editor** tab, choose **Choose a service**\.

1. From **AWS Services**, choose **IAM**\.

1. For **Actions**, enter **PassRole** for the **Filter actions** prompt, and choose **PassRole**\.

1. Expand the **Resources** section, choose **Add ARN**, paste the ARN for the Change Manager assume role you copied at the end of [Task 2: Creating an assume role for Change Manager](#change-manager-role), and then choose **Add**\.

1. Choose **Review policy**\.

1. For **Name**, enter a name to identify the policy, and then choose **Create policy**\.

You have finished configuring the required roles for Change Manager\. You can now use the Change Manager assume role ARN in your Change Manager operations\.