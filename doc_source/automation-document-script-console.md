# Creating a runbook that runs a script \(console\)<a name="automation-document-script-console"></a>

**To create a runbook that runs a script**

The following procedure describes how to use Document Builder in the AWS Systems Manager console to create a custom runbook that runs a script that you provide\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Create automation**\.

1. For **Name**, enter a descriptive name for the runbook\.

1. For **Document description**, provide the markdown style description for the runbook\. You can provide instructions for using the runbook, numbered steps, or any other type of information to describe the runbook\. Refer to the default text for information about formatting your content\.
**Tip**  
Toggle between **Hide preview** and **Show preview** to see what your description content looks like as you compose\.

1. \(Optional\) For **Assume role**, enter the name or Amazon Resource Name \(ARN\) of a service role to perform actions on your behalf\. If you don't specify a role, Automation uses the access permissions of the user who invokes the automation\.
**Important**  
For runbooks not owned by Amazon that use the `aws:executeScript` action, a role must be specified\. For information, see [Permissions for using runbooks](automation-document-script.md#execution-permissions)\.

1. \(Optional\) For **Outputs**, enter any outputs for the automation to make available for other processes\. 

   For example, if your runbook creates a new AMI, you might specify \["CreateImage\.ImageId"\], and then use this output to create new instances in a subsequent automation\.

1. \(Optional\) Expand the **Input parameters** section and do the following\.

   1. For **Parameter name**, enter a descriptive name for the runbook parameter you are creating\.

   1. For **Type**, choose a type for the parameter, such as String or StringList\.

   1. For **Required**, do one of the following\.
      + Choose **Yes** if a value for this runbook parameter must be supplied at runtime\.
      + Choose **No** if the parameter is not required, and \(optional\) enter a default parameter value in **Default value**\.

   1. For **Description**, enter a description for the runbook parameter\.
**Note**  
To add more runbook parameters, choose **Add a parameter**\. To remove a runbook parameter, choose the **X** \(Remove\) button\.

1. \(Optional\) Expand the **Target type** sections and choose a target type to define the kinds of resources the runbook can run on\. For example, to use a runbook on EC2 instances, choose `/AWS::EC2::Instance`\.
**Note**  
If you specify a value of '`/`', the runbook can run on all types of resources\. For a list of valid resource types, see [AWS Resource Types Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html) in the *AWS CloudFormation User Guide*\.

1. In the **Step 1** section, provide the following information\.
   + For **Step name**, enter a descriptive name for the first step of the automation\.
   + For **Action type**, select **Run a script** \(**aws:executeScript**\)\.
   + For **Description**, enter a description for the automation step\. You can use Markdown to format your text\. 

1. Expand the **Inputs** section and provide the following information\.
   + For **Runtime**, choose the type of script you are adding\. Currently, Automation supports Python 3\.6, Python 3\.7, and PowerShell Core 6\.0\.
   + For **Handler**, enter the the function name from your script\. \(Not required for PowerShell\.\)
**Important**  
You must ensure the function defined in the handler has two parameters, `events` and `context`\. For example, you would enter `launch_instance` if your script began with the following\.  

     ```
     def launch_instance(events, context):
       import boto3
       ec2 = boto3.client('ec2')
     
     [...truncated...]
     ```
   + For **Script**, choose a method to provide a script to the runbook\.
     + To embed a script within the runbook, enter the script code in the text\-box area\. 

       \-or\-
     + For **Attachment**, choose either **Stored on my machine** or **Upload S3 File URL**\.

       If you choose **Stored on my machine**: For Amazon S3 URL, enter the location of an S3 bucket in your account where you want to store the upload attachment, and then choose **Upload** to browse to and select the file\.

       If you choose **Upload S3 File URL**, provide the following information:
       + **S3 file url**: Enter the location in an S3 bucket in your account where the file is stored\.
       + **File name**: Enter the name of the file\.
       + **File checksum**: Enter the checksum of the file by using the sha256 algorithm\. 
**Tip**  
You can calculate the checksum of the file in sha256 by using a tool like shasum in Linux\. For example: 'shasum \-a 256 /path/to/file'\. In Windows, you can use the `Get-FileHash` PowerShell cmdlet to obtain the same information\. The ETag or md5 checksum won't work for this value\. 

1. \(Optional\) Expand **Additional inputs** and do the following\.
   + For **Input name**, chose InputPayload\. \- Function input in YAML format\. 
   + For **Input value**, enter your script input in YAML format\.

1. \(Optional\) Expand **Outputs** and enter the **Name**, **Selector**, and **Type** for any output to create from this step\. Step output can be used in subsequent steps of your runbook\. Here are a few examples for demonstration\.

   **Name**: **myInstance** \| **Selector**: **$\.InstanceInformationList\[0\]\.InstanceId** \| **Type**: **String**

   **Name**: `platform` \| **Selector**: `$.Reservations[0].Instances[0].Platform` \| **Type**: **String**

   **Name**: `message` \| **Selector**: $\.Payload\.message \| **Type**: **String**

   For more information about outputs, see [Working with inputs and outputs](automation-aws-apis-calling.md#automation-aws-apis-calling-input-output)\.
**Tip**  
To add more outputs, select **Add output**\. 

1. \(Optional\) Expand the **Common properties** section and specify properties for the actions that are common to all Automation actions\. For example, for **Timeout seconds**, you can provide a value in seconds to specify how long the step can run before it is stopped\.

   For more information, see [Properties shared by all actions](automation-actions.md#automation-common)\.
**Note**  
To add more steps, select **Add step** and repeat the procedure for creating a step\. To remove a step, choose **Remove step**\.

1. Choose **Create document** to save the runbook\.