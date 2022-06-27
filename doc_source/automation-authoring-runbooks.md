# Authoring Automation runbooks<a name="automation-authoring-runbooks"></a>

Each runbook in Automation, a capability of AWS Systems Manager, defines an automation\. Automation runbooks define the actions that are performed during an automation\. In the runbook content, you define the input parameters, outputs, and actions that Systems Manager performs on your managed instances and AWS resources\. 

Automation includes several pre\-defined runbooks that you can use to perform common tasks like restarting one or more Amazon Elastic Compute Cloud \(Amazon EC2\) instances or creating an Amazon Machine Image \(AMI\)\. However, your use cases might extend beyond the capabilities of the pre\-defined runbooks\. If this is the case, you can create your own runbooks and modify them to your needs\.

A runbook consists of automation actions, parameters for those actions, and input parameters that you specify\. A runbook's content is written in either YAML or JSON\. If you're not familiar with either YAML or JSON, we recommend using Document Builder, or learning more about either markup language before attempting to author your own runbook\. For more information about Document Builder, see [Using Document Builder to create a custom runbook](automation-walk-document-builder.md)\.

The following sections will help you author your first runbook\.

## Identify your use case<a name="automation-authoring-runbooks-use-case"></a>

The first step in authoring a runbook is identifying your use case\. For example, you scheduled the `AWS-CreateImage` runbook to run daily on all of your production Amazon EC2 instances\. At the end of the month, you decide you have more images than are necessary for recovery points\. Going forward, you want to automatically delete the oldest AMI of an Amazon EC2 instance when a new AMI is created\. To accomplish this, you create a new runbook that does the following:

1. Runs the `aws:createImage` action and specifies the instance ID in the image description\.

1. Runs the `aws:waitForAwsResourceProperty` action to poll the state of the image until it's `available`\.

1. After the image state is `available`, the `aws:executeScript` action runs a custom Python script that gathers the IDs of all images associated with your Amazon EC2 instance\. The script does this by filtering, using the instance ID in the image description you specified at creation\. Then, the script sorts the list of image IDs based on the `creationDate` of the image and outputs the ID of the oldest AMI\.

1. Lastly, the `aws:deleteImage` action runs to delete the oldest AMI using the ID from the output of the previous step\.

In this scenario, you were already using the `AWS-CreateImage` runbook but found that your use case required greater flexibility\. This is a common situation because there can be overlap between runbooks and automation actions\. As a result, you might have to adjust which runbooks or actions you use to address your use case\.

For example, the `aws:executeScript` and `aws:invokeLambdaFunction` actions both allow you to run custom scripts as part of your automation\. To choose between them, you might prefer `aws:invokeLambdaFunction` because of the additional supported runtime languages\. However, you might prefer `aws:executeScript` because it allows you to author your script content directly in YAML runbooks and provide script content as attachments for JSON runbooks\. You might also consider `aws:executeScript` to be simpler in terms of AWS Identity and Access Management \(IAM\) setup\. Because it uses the permissions provided in the `AutomationAssumeRole`, `aws:executeScript` doesn't require an additional AWS Lambda function execution role\.

In any given scenario, one action might provide more flexibility, or added functionality, over another\. Therefore, we recommend that you review the available input parameters for the runbook or action you want to use to determine which best fits your use case and preferences\.

## Set up your development environment<a name="automation-authoring-runbooks-environment"></a>

After you've identified your use case and the pre\-defined runbooks or automation actions you want to use in your runbook, it's time to set up your development environment for the content of your runbook\. To develop your runbook content, we recommend using the AWS Toolkit for Visual Studio Code instead of the Systems Manager Documents console\. 

The Toolkit for VS Code is an open\-source extension for Visual Studio Code \(VS Code\) that offers more features than the Systems Manager Documents console\. Helpful features include schema validation for both YAML and JSON, snippets for automation action types, and auto\-complete support for various options in both YAML and JSON\. 

For more information about installing the Toolkit for VS Code, see [Installing the AWS Toolkit for Visual Studio Code](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/setup-toolkit.html)\. For more information about using the Toolkit for VS Code to develop runbooks, see [Working with Systems Manager Automation documents](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/systems-manager-automation-docs.html) in the *AWS Toolkit for Visual Studio Code User Guide*\.

## Develop runbook content<a name="automation-authoring-runbooks-developing-content"></a>

With your use case identified and environment set up, you're ready to develop the content for your runbook\. Your use case and preferences will largely dictate the automation actions or runbooks you use in your runbook content\. Some actions support only a subset of input parameters when compared to another action that allows you to accomplish a similar task\. Other actions have specific outputs, such as `aws:createImage`, where some actions allow you to define your own outputs, such as `aws:executeAwsApi`\. 

If you're unsure how to use a particular action in your runbook, we recommend reviewing the corresponding entry for the action in the [Systems Manager Automation actions reference](automation-actions.md)\. We also recommend reviewing the content of pre\-defined runbooks to see real\-world examples of how these actions are used\. For more examples of real\-world applications of runbooks, see [Sample scenarios and custom runbook solutions](automation-document-samples.md)\.

To demonstrate the differences in simplicity and flexibility that runbook content provides, the following tutorials provide an example of how to patch groups of Amazon EC2 instances in stages:
+ [Example 1: Creating parent\-child runbooks](automation-authoring-runbooks-parent-child-example.md) – In this example, two runbooks are used in a parent\-child relationship\. The parent runbook initiates a rate control automation of the child runbook\. 
+ [Example 2: Scripted runbook](automation-authoring-runbooks-scripted-example.md) – This example demonstrates how you can accomplish the same tasks of Example 1 by condensing the content into a single runbook and using scripts in your runbook\.