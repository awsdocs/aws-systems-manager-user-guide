# Creating Automation documents using Document Builder<a name="automation-document-builder"></a>

If the AWS Systems Manager public Automation documents don't support all the actions you want to perform on your AWS resources, you can create your own documents\. To create a custom Automation document, you can manually create a local JavaScript Object Notation \(JSON\) or YAML format file with the appropriate automation actions\. Alternatively, you can use the Document Builder in the Systems Manager console to more easily build a custom document\.

Using the Document Builder, you can add Automation action steps to your custom document and provide the required parameters without having to use JSON or YAML syntax\. After you add steps and create the document, the system converts the actions you've added into the YAML format that Systems Manager can use to run automation workflows\.

Automation documents support the use of Markdown, a markup language, which allows you to add wiki\-style descriptions to documents and individual steps within the document\. For more information on using Markdown, see [Using Markdown in AWS](https://docs.aws.amazon.com/general/latest/gr/aws-markdown.html)\.

**Tip**  
This topic provides general information for using Document Builder with any supported action type\. For more information about creating Automation documents that run scripts, see the following topics:  
[Creating Automation documents that run scripts](automation-document-script.md) – Provides information for using Document Builder to create an Automation document that includes the `aws:executeScript` action\.
[Creating an Automation document that runs scripts \(command line\)](automation-document-script-commandline.md) – Provides information for using a command line tool to create an Automation document that runs a script\.
[ Walkthrough: Using Document Builder to create a custom Automation document](automation-walk-document-builder.md) – Provides step\-by\-step guidance for creating an Automation document that runs scripts to \(1\) launch an Amazon Elastic Compute Cloud \(EC2\) instance and \(2\) wait for the instance status to change to `ok`\.

**Before You Begin**  
Before you create a custom Automation document using Document Builder, we recommend that you read about the different actions that you can use within an Automation document\. For more information see, [Systems Manager Automation actions reference](automation-actions.md)\.

**To create an Automation document using Document Builder**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Create automation**\.

1. For **Name**, type a descriptive name for the document\.

1. For **Document description**, provide the markdown style description for the document\. You can provide instructions for using the document, numbered steps, or any other type of information to describe the document\. Refer to the default text for information about formatting your content\.
**Tip**  
Toggle between **Hide preview** and **Show preview** to see what your description content looks like as you compose\.

1. \(Optional\) For **Assume role**, enter the name or ARN of a service role to perform actions on your behalf\. If you don't specify a role, Automation uses the access permissions of the user who invokes the automation execution\.
**Important**  
For Automation documents not owned by Amazon that use the `aws:executeScript` action, a role must be specified\. For information, see [Permissions for running Automation executions](automation-document-script.md#execution-permissions)\.

1. \(Optional\) For **Outputs**, enter any outputs for the execution of this document to make available for other processes\. 

   For example, if your document creates a new AMI, you might specify \["CreateImage\.ImageId"\], and then use this output to create new instances in a subsequent automation execution\.

1. \(Optional\) Expand the **Input parameters** section and do the following\.

   1. For **Parameter name**, enter a descriptive name for the document parameter you are creating\.

   1. For **Type**, choose a type for the parameter, such as String or MapList\.

   1. For **Required**, do one of the following: 
      + Choose **Yes** if a value for this document parameter must be supplied at runtime\.
      + Choose **No** if the parameter is not required, and \(optional\) enter a default parameter value in **Default value**\.

   1. For **Description**, enter a description for the document parameter\.
**Note**  
To add more document parameters, choose **Add a parameter**\. To remove a document parameter, choose the **X** \(Remove\) button\.

1. \(Optional\) Expand the **Target type** section and choose a target type to define the kinds of resources the document can run on\. For example, to run a document on EC2 instances, choose `/AWS::EC2::Instance`\.
**Note**  
If you specify a value of '`/`', the document can run on all types of resources\. For a list of valid resource types, see [AWS Resource Types Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html) in the *AWS CloudFormation User Guide*\.

1. \(Optional\) Expand the **Document tags** section and enter one or more tag key\-value pairs to apply to the document\. Tags make it easier to identify, organize, and search for resources\. For more information, see [Tagging Systems Manager documents](tagging-documents.md)\.

1. In the **Step 1** section, provide the following information\.
   + For **Step name**, enter a descriptive name for the first step of the automation workflow\.
   + For **Action type**, select the action type to use for this step\.

     For a list and information about the available action types, see [Systems Manager Automation actions reference](automation-actions.md)\.
   + For **Description**, enter a description for the automation step\. You can use Markdown to format your text\.
   + Depending on the **Action type** selected, enter the required inputs for the action type in the **Step inputs** section\. For example, if you selected the action `aws:approve`, you must specify a value for the `Approvers` property\.

     For information about the step input fields, see the entry in [Systems Manager Automation actions reference](automation-actions.md) for the action type you selected\. For example: [aws:executeStateMachine – Run an AWS Step Functions state machine](automation-action-executeStateMachine.md)\.
   + \(Optional\) For **Additional inputs**, provide any additional input values needed for your document\. The available input types depend on the action type you selected for the step\. \(Note that some action types require input values\.\)
**Note**  
To add more inputs, choose **Add optional input**\. To remove an input, choose the **X** \(Remove\) button\.
   + \(Optional\) For **Outputs**, enter any outputs for the execution of this step to make available for other processes\.
**Note**  
**Outputs** isn't available for all action types\.
   + \(Optional\) Expand the **Common properties** section and specify properties for the actions that are common to all Automation actions\. For example, for **Timeout seconds**, you can provide a value in seconds to specify how long the step can run before it is stopped\.

     For more information, see [Properties shared by all actions](automation-actions.md#automation-common)\.
**Note**  
To add more document steps, select **Add step** and repeat the procedure for creating a step\. To remove a document step, choose **Remove step**\.

1. Choose **Create automation** to save the document\.