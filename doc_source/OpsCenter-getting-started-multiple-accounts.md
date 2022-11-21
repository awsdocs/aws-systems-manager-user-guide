# \(Optional\) Setting up OpsCenter to work with OpsItems across accounts<a name="OpsCenter-getting-started-multiple-accounts"></a>

OpsCenter supports working with OpsItems from a management or delegated administrator account and a member account during a session\. Once configured, users can perform the following types of actions:
+ Create, view, and update OpsItems in a member account\.
+ View detailed information about AWS resources that are specified in OpsItems in a member account\.
+ Start Systems Manager Automation runbooks to remediate issues with AWS resources in a member account\.

**Note**  
Note the following information about working with OpsItems across accounts:  
Only OpsItems of [type](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateOpsItem.html#systemsmanager-CreateOpsItem-request-OpsItemType) `/aws/issue` are supported when working in OpsCenter across accounts\.
OpsCenter and Explorer, a capabilty of Systems Manager, share an integrated setup experience\. If you haven't already done so while setting up Explorer, we recommend you create a delegated administrator account\. Specifying one AWS account as a delegated administrator can improve security for several administrator tasks, including working with OpsItems across accounts\. For more information, see [Configuring a delegated administrator](Explorer-setup-delegated-administrator.md)\.

**Before you begin**  
You must have at least one organization set up and configured in Organizations\. You perform the tasks in this section in an AWS account that is serving as the *management account* in Organizations\. For more information, see [Creating and managing an organization](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_org.html) in the *AWS Organizations User Guide*\.

## Configuring access and permission for working with OpsItems across accounts<a name="OpsCenter-getting-started-multiple-accounts-onboarding"></a>

Use the procedures in this section to enable the Systems Manager service principal in Organizations and configure AWS Identity and Access Management \(IAM\) permissions for working with OpsItems across accounts\. This section includes the following tasks:

**Topics**
+ [Task 1: Create a resource data sync](#OpsCenter-getting-started-multiple-accounts-onboarding-rds)
+ [Task 2: Enable the Systems Manager service principal in AWS Organizations](#OpsCenter-getting-started-multiple-accounts-onboarding-service-principal)
+ [Task 3: Create the `AWSServiceRoleForAmazonSSM_AccountDiscovery` service\-linked role](#OpsCenter-getting-started-multiple-accounts-onboarding-SLR)
+ [Task 4: Configure permissions to work with OpsItems across accounts](#OpsCenter-getting-started-multiple-accounts-onboarding-resource-policy)
+ [Task 5: Configure permissions to work with related resources across accounts](#OpsCenter-getting-started-multiple-accounts-onboarding-related-resources-permissions)

### Task 1: Create a resource data sync<a name="OpsCenter-getting-started-multiple-accounts-onboarding-rds"></a>

After you set up and configure AWS Organizations, you can aggregate OpsItems in OpsCenter for an entire organization by creating a resource data sync\.

For information about creating a resource data sync, see [Creating a resource data sync](Explorer-resource-data-sync.md#Explorer-resource-data-sync-configuring-multi) in the Explorer documentation\. In the **Add accounts** section, choose **Include all accounts from my AWS Organizations configuration**\.

### Task 2: Enable the Systems Manager service principal in AWS Organizations<a name="OpsCenter-getting-started-multiple-accounts-onboarding-service-principal"></a>

To enable a user to work with OpsItems across accounts, the Systems Manager service principal must be enabled in AWS Organizations\. If you previously configured Systems Manager for multi\-account scenarios using other capabilities, the Systems Manager service principal might already be configured in Organizations\. Run the following commands from the AWS Command Line Interface \(AWS CLI\) to verify\. If you *haven't* configured Systems Manager for other multi\-account scenarios, skip to the next procedure, *Enable the Systems Manager service principal in AWS Organizations*\.

**To verify the Systems Manager service principal is enabled in AWS Organizations**

1. Sign in to the AWS Organizations management account\.

1. [Download](https://aws.amazon.com/cli/) the latest version of the AWS CLI to your local machine\.

1. Open the AWS CLI, and run the following command to specify your credentials and an AWS Region\.

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

1. Run the following command to verify that the Systems Manager service principal is enabled for AWS Organizations\.

   ```
   aws organizations list-aws-service-access-for-organization
   ```

   The command returns information similar to that shown in the following example\.

   ```
   {
       "EnabledServicePrincipals": [
           {
               "ServicePrincipal": "member.org.stacksets.cloudformation.amazonaws.com",
               "DateEnabled": "2020-12-11T16:32:27.732000-08:00"
           },
           {
               "ServicePrincipal": "opsdatasync.ssm.amazonaws.com",
               "DateEnabled": "2022-01-19T12:30:48.352000-08:00"
           },
           {
               "ServicePrincipal": "ssm.amazonaws.com",
               "DateEnabled": "2020-12-11T16:32:26.599000-08:00"
           }
       ]
   }
   ```

**To enable the Systems Manager service principal in AWS Organizations**

If you haven't previously configured the Systems Manager service principal for Organizations, use the following procedure to do so\. For more information about this command, see [enable\-aws\-service\-access](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/organizations/enable-aws-service-access.html) in the *AWS CLI Command Reference*\.

1. Sign in to the AWS Organizations management account\.

1. [Download](https://aws.amazon.com/cli/) the latest version of the AWS CLI to your local machine\.

1. Open the AWS CLI, and run the following command to specify your credentials and an AWS Region\.

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

1. Run the following command to enable the Systems Manager service principal for AWS Organizations\.

   ```
   aws organizations enable-aws-service-access --service-principal "ssm.amazonaws.com"
   ```

### Task 3: Create the `AWSServiceRoleForAmazonSSM_AccountDiscovery` service\-linked role<a name="OpsCenter-getting-started-multiple-accounts-onboarding-SLR"></a>

A service\-linked role such as the `AWSServiceRoleForAmazonSSM_AccountDiscovery` role is a unique type of IAM role that is linked directly to an AWS service, such as Systems Manager\. Service\-linked roles are predefined by the service and include all the permissions that the service requires to call other AWS services on your behalf\. For more information about the `AWSServiceRoleForAmazonSSM_AccountDiscovery` service\-linked role, see [Service\-linked role permissions for Systems Manager account discovery](using-service-linked-roles-service-action-2.md#service-linked-role-permissions-service-action-2)\.

Use the following procedure to create the `AWSServiceRoleForAmazonSSM_AccountDiscovery` service\-linked role by using the AWS CLI\. For more information about the command used in this procedure, see [create\-service\-linked\-role](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/iam/create-service-linked-role.html) in the *AWS CLI Command Reference\.*

**To create the `AWSServiceRoleForAmazonSSM_AccountDiscovery` service\-linked role**

1. Sign in to the AWS Organizations management account\.

1. While signed in to the Organizations management account, run the following command\.

   ```
   aws iam create-service-linked-role \
       --aws-service-name accountdiscovery.ssm.amazonaws.com \
       --description "Systems Manager account discovery for AWS Organizations service-linked role"
   ```

### Task 4: Configure permissions to work with OpsItems across accounts<a name="OpsCenter-getting-started-multiple-accounts-onboarding-resource-policy"></a>

Use AWS CloudFormation stacksets to create an `OpsItemGroup` resource policy and an IAM execution role that give users permissions to work with OpsItems across accounts\. To get started, download and unzip the [https://docs.aws.amazon.com/systems-manager/latest/userguide/samples/OpsCenterCrossAccountMembers.zip](https://docs.aws.amazon.com/systems-manager/latest/userguide/samples/OpsCenterCrossAccountMembers.zip) file\. This file contains the `OpsCenterCrossAccountMembers.yaml` AWS CloudFormation template file\. When you create a stack set by using this template, CloudFormation automatically creates the `OpsItemCrossAccountResourcePolicy` resource policy and the `OpsItemCrossAccountExecutionRole` execution role in the account\. For more information about creating a stack set, see [Create a stack set](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-getting-started-create.html) in the *AWS CloudFormation User Guide*\.

**Important**  
Note the following important information about this task:  
You must deploy the stackset while logged into the AWS Organizations management account\.
You must repeat this procedure while logged into *every* account that you want to *target* for working with OpsItems across accounts, including the delegated administrator account\.
If you want to enable cross\-account OpsItems administration in different AWS Regions, choose **Add all regions** in the **Specify regions** section of the template\. Cross\-account OpsItem administration isn't supported for opt\-in Regions\.

### Task 5: Configure permissions to work with related resources across accounts<a name="OpsCenter-getting-started-multiple-accounts-onboarding-related-resources-permissions"></a>

The **Related resources** tab in an OpsItem can include detailed information about impacted resources such as Amazon Elastic Compute Cloud \(Amazon EC2\) instances or Amazon Simple Storage Service \(Amazon S3\) buckets\. The `OpsItemCrossAccountExecutionRole` execution role, which you created in the previous task, provides OpsCenter with read\-only permissions for member accounts to view related resources\. You must also create an IAM role to provide management accounts with permission to view and interact with related resources, which you will complete in this task\. 

To get started, download and unzip the [https://docs.aws.amazon.com/systems-manager/latest/userguide/samples/OpsCenterCrossAccountManagementRole.zip](https://docs.aws.amazon.com/systems-manager/latest/userguide/samples/OpsCenterCrossAccountManagementRole.zip) file\. This file contains the `OpsCenterCrossAccountManagementRole.yaml` AWS CloudFormation template file\. When you create a stack by using this template, CloudFormation automatically creates the `OpsCenterCrossAccountManagementRole` IAM role in the account\. For more information about creating a stack, see [Creating a stack on the AWS CloudFormation console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html) in the *AWS CloudFormation User Guide*\.

**Important**  
Note the following important information about this task:  
If you plan to specify an account as a delegated administrator for OpsCenter, be sure to specify that AWS account when you create the stack\. 
You must perform this procedure while logged into the AWS Organizations management account and again while logged into the delegated administrator account\.

For information about creating, viewing, and updating OpsItems across accounts, see [Working with OpsItems across accounts](OpsCenter-working-with-multiple-accounts.md)\.