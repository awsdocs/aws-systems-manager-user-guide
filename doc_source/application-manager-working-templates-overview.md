# Working with CloudFormation templates<a name="application-manager-working-templates-overview"></a>

Application Manager, a capability of AWS Systems Manager, includes a template library and other tools to help you manage AWS CloudFormation templates\. This section includes the following information\.

**Topics**
+ [Working with the template library](#application-manager-working-stacks-template-library-working)
+ [Creating a template](#application-manager-working-stacks-creating-template)
+ [Editing a template](#application-manager-working-stacks-editing-template)

## Working with the template library<a name="application-manager-working-stacks-template-library-working"></a>

The Application Manager template library provides tools to help you view, create, edit, delete, and clone templates\. You can also provision stacks directly from the template library\. Templates are stored as Systems Manager \(SSM\) documents of type `CloudFormation`\. By storing templates as SSM documents, you can use version controls to work with different versions of a template\. You can also set permissions and share templates\. After you successfully provision a stack, the stack and template are available in Application Manager and CloudFormation\. 

**Before you begin**  
We recommend that you read the following topics to learn more about SSM documents before you start working with CloudFormation templates in Application Manager\.
+ [AWS Systems Manager documents](sysman-ssm-docs.md)
+ [Sharing SSM documents](ssm-sharing.md)
+ [Best practices for shared SSM documents](ssm-before-you-share.md)

**To view the template library in Application Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1. In the **Applications** section, choose **CloudFormation stacks**\.

1. Choose **Template library**\.

## Creating a template<a name="application-manager-working-stacks-creating-template"></a>

The following procedure describes how to create a CloudFormation template in Application Manager\. When you create a template, you enter the stack details of the template in either JSON or YAML\. If you're unfamiliar with JSON or YAML, you can use AWS CloudFormation Designer, a tool for visually creating and modifying templates\. For more information, see [What is AWS CloudFormation Designer?](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/working-with-templates-cfn-designer.html) in the *AWS CloudFormation User Guide*\. For information about the structure and syntax of a template, see [Template anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html)\.

You can also construct a template from multiple template snippets\. Template snippets are examples that demonstrate how to write templates for a particular resource\. For example, you can view snippets for Amazon Elastic Compute Cloud \(Amazon EC2\) instances, Amazon Simple Storage Service \(Amazon S3\) domains, AWS CloudFormation mappings, and more\. Snippets are grouped by resource\. You can find general\-purpose AWS CloudFormation snippets in the [General template snippets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-general.html) section of the *AWS CloudFormation User Guide*\. 

### Creating a CloudFormation template in Application Manager \(console\)<a name="application-manager-working-stacks-creating-template-console"></a>

Use the following procedure to create a CloudFormation template in Application Manager by using the AWS Management Console\.

**To create a CloudFormation template in Application Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1. In the **Applications** section, choose **CloudFormation stacks**\.

1. Choose **Template library**, and then either choose **Create template** or choose an existing template and then choose **Actions**, **Clone**\.

1. For **Name**, enter a name for the template that helps you identify the resources it creates or the purpose of the stack\.

1. \(Optional\) For **Version name**, enter a name or a number to identity the template version\.

1. \(Optional\) For **Description**, enter information about this template\.

1. In the **Code editor** section, choose either **YAML** or **JSON** and then either enter or copy and paste your template code\.

1. \(Optional\) In the **Tags** section, apply one or more tag key name/value pairs to the template\.

   Tags are optional metadata that you assign to a resource\. By using tags, you can categorize a resource in different ways, such as by purpose, owner, or environment\. For more information about tagging Systems Manager resources, see [Tagging Systems Manager resources](tagging-resources.md)\.

1. \(Optional\) In the **Permissions** section, enter an AWS account ID and choose **Add account**\. This action provides read permission to the template\. The account owner can provision and clone the template, but they can't edit or delete it\. 

1. Choose **Create**\. The template is saved in the Systems Manager \(SSM\) Document service\.

### Creating a CloudFormation template in Application Manager \(command line\)<a name="application-manager-working-stacks-creating-template-cli"></a>

After you create the content of your CloudFormation template in JSON or YAML, you can use the AWS Command Line Interface \(AWS CLI\) or AWS Tools for PowerShell to save the template as an SSM document\.

**Before you begin**  
Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\. For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

------
#### [ Linux & macOS ]

```
aws ssm create-document \
    --content file://path/to/template_in_json_or_yaml \
    --name "a_name_for_the_template" \
    --document-type "CloudFormation" \
    --document-format "JSON_or_YAML" \
    --tags "Key=tag-key,Value=tag-value"
```

------
#### [ Windows ]

```
aws ssm create-document ^
    --content file://C:\path\to\template_in_json_or_yaml ^
    --name "a_name_for_the_template" ^
    --document-type "CloudFormation" ^
    --document-format "JSON_or_YAML" ^
    --tags "Key=tag-key,Value=tag-value"
```

------
#### [ PowerShell ]

```
$json = Get-Content -Path "C:\path\to\template_in_json_or_yaml | Out-String
New-SSMDocument `
    -Content $json `
    -Name "a_name_for_the_template" `
    -DocumentType "CloudFormation" `
    -DocumentFormat "JSON_or_YAML" `
    -Tags "Key=tag-key,Value=tag-value"
```

------

If successful, the command returns a response similar to the following\.

```
{
    "DocumentDescription": {
        "Hash": "c1d9640f15fbdba6deb41af6471d6ace0acc22f213bdd1449f03980358c2d4fb",
        "HashType": "Sha256",
        "Name": "MyTestCFTemplate",
        "Owner": "428427166869",
        "CreatedDate": "2021-06-04T09:44:18.931000-07:00",
        "Status": "Creating",
        "DocumentVersion": "1",
        "Description": "My test template",
        "PlatformTypes": [],
        "DocumentType": "CloudFormation",
        "SchemaVersion": "1.0",
        "LatestVersion": "1",
        "DefaultVersion": "1",
        "DocumentFormat": "YAML",
        "Tags": [
            {
                "Key": "Templates",
                "Value": "Test"
            }
        ]
    }
```

## Editing a template<a name="application-manager-working-stacks-editing-template"></a>

Use the following procedure to edit a CloudFormation template in Application Manager\. Template changes are available in CloudFormation after you provision a stack that uses the updated template\.

**To edit a CloudFormation template in Application Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1. In the **Applications** section, choose **CloudFormation stacks**\.

1. Choose **Template library**\.

1. Choose a template, and then choose **Actions**, **Edit**\. You can't change the name of a template, but you can change all other details\.

1. Choose **Save**\. The template is saved in the Systems Manager Document service\.