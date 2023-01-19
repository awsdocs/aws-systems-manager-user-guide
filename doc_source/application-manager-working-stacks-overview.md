# Working with CloudFormation stacks<a name="application-manager-working-stacks-overview"></a>

Application Manager, a capability of AWS Systems Manager, helps you provision and manage resources for your applications by integrating with AWS CloudFormation\. You can create, edit, and delete CloudFormation templates and stacks in Application Manager\. A *stack* is a collection of AWS resources that you can manage as a single unit\. This means you can create, update, or delete a collection of AWS resources by using CloudFormation stacks\. A *template* is a formatted text file in JSON or YAML that specifies the resources you want to provision in your stacks\. This section includes the following information\.

**Topics**
+ [Creating a stack](#application-manager-working-stacks-creating-stack)
+ [Updating a stack](#application-manager-working-stacks-editing-stack)

## Creating a stack<a name="application-manager-working-stacks-creating-stack"></a>

The following procedures describe how to create a CloudFormation stack by using Application Manager\. A stack is based on a template\. When you create a stack, you can either choose an existing template or create a new one\. After you create the stack, the system immediately attempts to create the resources identified in the stack\. After the system successfully provisions the resources, the template and the stack are available to view and edit in Application Manager and CloudFormation\.

**Note**  
There is no charge to use Application Manager to create a stack, but you are charged for AWS resources created in the stack\. 

### Creating a CloudFormation stack by using Application Manager \(console\)<a name="application-manager-working-stacks-creating-stack-console"></a>

Use the following procedure to create a stack by using Application Manager in the AWS Management Console\.

**To create a CloudFormation stack**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1. In the **Applications** section, choose **CloudFormation stacks**\.

1. In the **Prepare a template** section, choose an option\. If you choose **Use an existing template**, you can use the tabs in the **Choose a template** section to locate the template you want\. If you choose one of the other options, complete the wizard to prepare a template\.

1. On the **Specify template details** page, verify the details of the template to ensure the process creates the resources you want\.
   + \(Optional\) In the **Tags** section, apply one or more tag key name/value pairs to the template\.
   + Tags are optional metadata that you assign to a resource\. By using tags, you can categorize a resource in different ways, such as by purpose, owner, or environment\. For more information about tagging Systems Manager resources, see [Tagging Systems Manager resources](tagging-resources.md)\.
   + Choose **Next**\.

1. On the **Edit stack details** page, for **Stack name**, enter a name that helps you identify the resources created by the stack or its purpose\.
   + The **Parameters** section includes all optional and required parameters specified in the template\. Enter one or more parameters in each field\.
   + \(Optional\) In the **Tags** section, apply one or more tag key name/value pairs to the stack\.
   + \(Optional\) In the **Permissions** section, specify an AWS Identity and Access Management \(IAM\) role name or an IAM Amazon Resource Name \(ARN\)\. The system uses the specified service role to create all resources specified in your stack\. If you don't specify an IAM role, then AWS CloudFormation uses a temporary session that the system generates from your user credentials\. For more information about this IAM role, see [AWS CloudFormation service role](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-iam-servicerole.html) in the *AWS CloudFormation User Guide*\. 
   + Choose **Next**\.

1. On the **Review and provision** page, review all the details of the stack\. Choose an **Edit** button on this page to make change\.

1. Choose **Provision stack**\.

Application Manager displays the **CloudFormation stacks** page and the status of the stack creation and deployment\. If CloudFormation fails to create and provision the stack, see the following topics in the *AWS CloudFormation User Guide*\. 
+ [Stack status codes](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-describing-stacks.html#w2ab1c23c15c17c11)
+ [Troubleshooting AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html)

After your stack resources are provisioned and running, users can edit resources directly by using the underlying service that created the resource\. For example, a user can use the Amazon Elastic Compute Cloud \(Amazon EC2\) console to update a server instance that was created as part of a CloudFormation stack\. Some changes may be accidental, and some may be made intentionally to respond to time\-sensitive operational events\. Regardless, changes made outside of CloudFormation can complicate stack update or deletion operations\. You can use drift detection or *drift status* to identify stack resources to which configuration changes have been made outside of CloudFormation management\. For information about drift status, see [Detecting unmanaged configuration changes to stacks and resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-stack-drift.html)\.

### Creating a CloudFormation stack by using Application Manager \(command line\)<a name="application-manager-working-stacks-creating-stack-cli"></a>

Use the following AWS Command Line Interface \(AWS CLI\) procedure to provision a stack by using a CloudFormation template that is stored as an SSM document in Systems Manager\. Replace each *example resource placeholder* with your own information\. For information about other AWS CLI procedures for creating stacks, see [Creating a stack](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-cli-creating-stack.html) in the *AWS CloudFormation User Guide*\. 

**Before you begin**  
Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\. For information, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) and [Installing the AWS Tools for PowerShell](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up.html)\.

------
#### [ Linux & macOS ]

```
aws cloudformation create-stack \
    --stack-name a_name_for_the_stack \
    --template-url "ssm-doc://arn:aws:ssm:Region:account_ID:document/template_name" \
```

------
#### [ Windows ]

```
aws cloudformation create-stack ^
     --stack-name a_name_for_the_stack ^
     --template-url "ssm-doc://arn:aws:ssm:Region:account_ID:document/template_name" ^
```

------
#### [ PowerShell ]

```
New-CFNStack `
    -StackName "a_name_for_the_stack" `
    -TemplateURL "ssm-doc://arn:aws:ssm:Region:account_ID:document/template_name" `
```

------

## Updating a stack<a name="application-manager-working-stacks-editing-stack"></a>

You can deploy updates to a CloudFormation stack by directly editing the stack in Application Manager\. With a direct update, you specify updates to a template or input parameters\. After you save and deploy the changes, CloudFormation updates the AWS resources according to the changes you specified\.

You can preview the changes that CloudFormation will make to your stack before you update it by using change sets\. For more information, see [Updating stacks using change sets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-changesets.html) in the *AWS CloudFormation User Guide*\. 

**To update a CloudFormation stack in Application Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1. In the **Applications** section, choose **CloudFormation stacks**\.

1. Choose a stack in the list and then choose **Actions**, **Update stack**\.

1. On the **Specify template source** page, choose one of the following options, and then choose **Next**\.
   + Choose **Use the template code currently provisioned in the stack** to view a template\. Choose a template version in the **Versions** list, and then choose **Next**\.
   + Choose **Switch to a different template** to choose or create a new template for the stack\.

1. After you finish making changes to the template, choose **Next**\.

1. On the **Edit stack details** page, you can edit parameters, tags, and permissions\. You can't change the stack name\. Make your changes and choose **Next**\.

1. On the **Review and provision** page, review all the details of the stack, and then choose **Provision stack**\.