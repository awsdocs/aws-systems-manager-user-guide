# Working with AWS CloudFormation templates and stacks in Application Manager<a name="application-manager-working-stacks"></a>

Application Manager, a capability of AWS Systems Manager, helps you provision and manage resources for your applications by integrating with AWS CloudFormation\. You can create, edit, and delete AWS CloudFormation templates and stacks in Application Manager\. A *stack* is a collection of AWS resources that you can manage as a single unit\. This means you can create, update, or delete a collection of AWS resources by using CloudFormation stacks\. A *template* is a formatted text file in JSON or YAML that specifies the resources you want to provision in your stacks\. 

Application Manager also includes a template library where you can clone, create, and store templates\. Application Manager and CloudFormation display the same information about the current status of a stack\. Templates and template updates are stored in Systems Manager until you provision the stack, at which time the changes are also displayed in CloudFormation\.

After you create a stack in Application Manager, the **CloudFormation stacks** page displays helpful information about it\. This includes the template used to create it, a count of [OpsItems](https://docs.aws.amazon.com/systems-manager/latest/userguide/OpsCenter.html) for resources in your stack, the [stack status](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-view-stack-data-resources.html#cfn-console-view-stack-data-resources-status-codes), and [drift status](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-stack-drift.html)\.

## Before you begin<a name="application-manager-working-stacks-before-you-begin"></a>

Use the following links to learn about CloudFormation concepts before you create, edit, or delete CloudFormation templates and stacks by using Application Manager\.
+ [What is AWS CloudFormation?](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)
+ [AWS CloudFormation best practices](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/best-practices.html)
+ [Learn template basics](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/gettingstarted.templatebasics.html)
+ [Working with AWS CloudFormation stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacks.html)
+ [Working with AWS CloudFormation templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-guide.html)
+ [Sample templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-sample-templates.html)

**Topics**
+ [Before you begin](#application-manager-working-stacks-before-you-begin)
+ [Working with CloudFormation templates](application-manager-working-templates-overview.md)
+ [Working with CloudFormation stacks](application-manager-working-stacks-overview.md)