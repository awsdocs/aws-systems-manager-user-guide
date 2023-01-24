# Setting up Change Manager for an organization \(management account\)<a name="change-manager-organization-setup"></a>

The tasks in this topic apply if you're using Change Manager, a capability of AWS Systems Manager, with an organization that is set up in AWS Organizations\. If you want to use Change Manager only with a single AWS account, skip to the topic [Configuring Change Manager options and best practices](change-manager-account-setup.md)\.

Perform the tasks in this section in an AWS account that is serving as the *management account* in Organizations\. For information about the management account and other Organizations concepts, see [AWS Organizations terminology and concepts](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_getting-started_concepts.html)\.

If you need to turn on Organizations and specify your account as the management account before proceeding, see [Creating and managing an organization](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_org.html) in the *AWS Organizations User Guide*\. 

**Note**  
This setup process can't be performed in the following AWS Regions:  
Europe \(Milan\) \(eu\-south\-1\)
Middle East \(Bahrain\) \(me\-south\-1\)
Africa \(Cape Town\) \(af\-south\-1\)
Asia Pacific \(Hong Kong\) \(ap\-east\-1\)
Ensure that you're working in a different Region in your management account for this procedure\.

During the setup procedure, you perform the following major tasks in Quick Setup, a capability of AWS Systems Manager\.
+ **Task 1: Register the delegated administrator account for your organization**

  The change\-related tasks that are performed using Change Manager are managed in one of your member accounts, which you specify to be the *delegated administrator account*\. The delegated administrator account you register for Change Manager becomes the delegated administrator account for all your Systems Manager operations\. \(You might have delegated administrator accounts for other AWS services\)\. Your delegated administrator account for Change Manager, which isn't the same as your management account, manages change activities across your organization, including change templates, change requests, and approvals for each\. In the delegated administrator account, you also specify other configuration options for your Change Manager operations\. 
**Important**  
The delegated administrator account must be the only member of the organizational unit \(OU\) to which it's assigned in Organizations\.
+ **Task 2: Define and specify runbook access policies for change requester roles, or custom job functions, that you want to use for your Change Manager operations**

  In order to create change requests in Change Manager, users in your member accounts must be granted AWS Identity and Access Management \(IAM\) permissions that allow them to access only the Automation runbooks and change templates you choose to make available to them\. 
**Note**  
When a user creates a change request, they first select a change template\. This change template might make multiple runbooks available, but the user can select only one runbook for each change request\. Change templates can also be configured to allow users to include any available runbook in their requests\.

  To grant the needed permissions, Change Manager uses the concept of *job functions*, which is also used by IAM\. However, unlike the [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in IAM, you specify both the names of your Change Manager job functions and the IAM permissions for those job functions\. 

  When you configure a job function, we recommend creating a custom policy and providing only the permissions needed to perform change management tasks\. For instance, you might specify permissions that limit users to that specific set of runbooks based on *job functions* that you define\. 

  For example, you might create a job function with the name `DBAdmin`\. For this job function, you might grant only permissions needed for runbooks related to Amazon DynamoDB databases, such as `AWS-CreateDynamoDbBackup` and `AWSConfigRemediation-DeleteDynamoDbTable`\. 

  As another example, you might want to grant some users only the permissions needed to work with runbooks related to Amazon Simple Storage Service \(Amazon S3\) buckets, such as `AWS-ConfigureS3BucketLogging` and `AWSConfigRemediation-ConfigureS3BucketPublicAccessBlock`\. 

  The configuration process in Quick Setup for Change Manager also makes a set of full Systems Manager administrative permissions available for you to apply to an administrative role you create\. 

  Each Change Manager Quick Setup configuration you deploy creates a job function in your delegated administrator account with permissions to run Change Manager templates and Automation runbooks in the organizational units you have selected\. You can create up to 15 Quick Setup configurations for Change Manager\. 
+ **Task 3: Choose which member accounts in your organization to use with Change Manager**

  You can use Change Manager with all the member accounts in all your organizational units that are set up in Organizations, and in all the AWS Regions they operate in\. If you prefer, you can instead use Change Manager with only some of your organizational units\.

**Important**  
We strongly recommend, before you begin this procedure, that you read through its steps to understand the configuration choices you're making and the permissions you're granting\. In particular, plan the custom job functions you will create and the permissions you assign to each job function\. This ensures that when later you attach the job function policies you create to individual users, user groups, or IAM roles, they're being granted only the permissions you intend for them to have\.  
As a best practice, begin by setting up the delegated administrator account using the login for an AWS account administrator\. Then configure job functions and their permissions after you have created change templates and identified the runbooks that each one uses\.

To set up Change Manager for use with an organization, perform the following task in the Quick Setup area of the Systems Manager console\.

You repeat this task for each job function you want to create for your organization\. Each job function you create can have permissions for a different set of organizational units\.

**To set up an organization for Change Manager in the Organizations management account**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Quick Setup**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Quick Setup** in the navigation pane\.

1. On the **Change Manager** card, choose **Create**\.

1. For **Delegated administrator account**, enter the ID of the AWS account you want to use for managing change templates, change requests, and runbook workflows in Change Manager\. 

   If you have previously specified a delegated administrator account for Systems Manager, its ID is already reported in this field\. 
**Important**  
The delegated administrator account must be the only member of the organizational unit \(OU\) to which it's assigned in Organizations\.  
If the delegated administrator account you register is later deregistered from that role, the system removes its permissions for managing Systems Manager operations at the same time\. Keep in mind that it will be necessary for you return to Quick Setup, designate a different delegated administrator account, and specify all job functions and permissions again\.  
If you use Change Manager across an organization, we recommend always making changes from the delegated administrator account\. While you can make changes from other accounts in the organization, those changes won't be reported in or viewable from the delegated administrator account\.

1. In the **Permissions to request and make changes** section, do the following\.
**Note**  
Each deployment configuration you create provides the permissions policy for just one job function\. You can return to Quick Setup later to create more job functions when you have created change templates to use in your operations\.

   **To create an administrative role** – For an administrator job function that has IAM permissions for all AWS actions, do the following\.
**Important**  
Granting users full administrative permissions should be done sparingly, and only if their roles require full Systems Manager access\. For important information about security considerations for Systems Manager access, see [Identity and access management for AWS Systems Manager](security-iam.md) and [Security best practices for Systems Manager](security-best-practices.md)\.

   1. For **Job function**, enter a name to identify this role and its permissions, such as **MyAWSAdmin**\.

   1. For **Role and permissions option**, choose **Administrator permissions**\.

   **To create other job functions** – To create a non\-administrative role, do the following:

   1. For **Job function**, enter a name to identify this role and suggest its permissions\. The name you choose should represent scope of the runbooks for which you will provide permissions, such as `DBAdmin` or `S3Admin`\. 

   1. For **Role and permissions option**, choose **Custom permissions**\.

   1. In the **Permissions policy editor**, enter the IAM permissions, in JSON format, to grant to this job function\.
**Tip**  
We recommend that you use the IAM policy editor to construct your policy and then paste the policy JSON into the **Permissions policy** field\.

**Sample policy: DynamoDB database management**  
For example, you might begin with policy content that provides permissions for working with the Systems Manager documents \(SSM documents\) the job function needs access to\. Here is a sample policy content that grants access to all the AWS managed Automation runbooks related to DynamoDB databases and two change templates that have been created in the sample AWS account `123456789012`, in the US East \(Ohio\) Region \(`us-east-2`\)\. 

   The policy also includes permission for the [StartChangeRequestExecution](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_StartChangeRequestExecution.html) operation, which is required for creating a change request in Change Calendar\. 
**Note**  
This example isn't comprehensive\. Additional permissions might be needed for working with other AWS resources, such as databases and nodes\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "ssm:CreateDocument",
                   "ssm:DescribeDocument",
                   "ssm:DescribeDocumentParameters",
                   "ssm:DescribeDocumentPermission",
                   "ssm:GetDocument",
                   "ssm:ListDocumentVersions",
                   "ssm:ModifyDocumentPermission",
                   "ssm:UpdateDocument",
                   "ssm:UpdateDocumentDefaultVersion"
               ],
               "Resource": [
                   "arn:aws:ssm:region:*:document/AWS-CreateDynamoDbBackup",
                   "arn:aws:ssm:region:*:document/AWS-AWS-DeleteDynamoDbBackup",
                   "arn:aws:ssm:region:*:document/AWS-DeleteDynamoDbTableBackups",
                   "arn:aws:ssm:region:*:document/AWSConfigRemediation-DeleteDynamoDbTable",
                   "arn:aws:ssm:region:*:document/AWSConfigRemediation-EnableEncryptionOnDynamoDbTable",
                   "arn:aws:ssm:region:*:document/AWSConfigRemediation-EnablePITRForDynamoDbTable",
                   "arn:aws:ssm:region:123456789012:document/MyFirstDBChangeTemplate",
                   "arn:aws:ssm:region:123456789012:document/MySecondDBChangeTemplate"
               ]
           },
           {
               "Effect": "Allow",
               "Action": "ssm:ListDocuments",
               "Resource": "*"
           },
           {
               "Effect": "Allow",
               "Action": "ssm:StartChangeRequestExecution",
               "Resource": "arn:aws:ssm:us-east-2:123456789012:automation-definition/*:*"
           }
       ]
   }
   ```

   For more information about IAM policies, see [Access management for AWS resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) and [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide\.*

1. In the **Targets** section, choose whether to grant permissions for the job function you're creating to your entire organization or only some of your organizational units\.

   If you choose **Entire organization**, continue to step 9\.

   If you choose **Custom**, continue to step 8\.

1. In the **Target OUs** section, select the check boxes of the organizational units to use with Change Manager\.

1. Choose **Create**\.

After the system finishes setting up Change Manager for your organization, it displays a summary of your deployments\. This summary information includes the name of the permissions policy that was created for the job function you configured\. For example, `AWS-QuickSetup-SSMChangeMgr-DBAdminInvocationRole`\. Make a note of this policy name and attach it to the users, groups, or IAM roles who will perform this job function\. For information about attaching IAM policies, see [Changing permissions for an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) in the *IAM User Guide*\.

**Note**  
Quick Setup uses AWS CloudFormation StackSets to deploy your configurations\. You can also view information about a completed deployment configuration in the AWS CloudFormation console\. For information about StackSets, see [Working with AWS CloudFormation StackSets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/what-is-cfnstacksets.html) in the *AWS CloudFormation User Guide*\.

Your next step is to configure additional Change Manager options\. You can complete this task in either your delegated administrator account or any account in an organization unit that you have allowed for use with Change Manager\. You configure options such as choosing a user identity management option, specifying which users can review and approve or reject change templates and change requests, and choosing which best practice options to allow for your organization\. For information, see [Configuring Change Manager options and best practices](change-manager-account-setup.md)\.