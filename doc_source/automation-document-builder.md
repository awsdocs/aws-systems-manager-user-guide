# Using Document Builder to create runbooks<a name="automation-document-builder"></a>

If the AWS Systems Manager public runbooks don't support all the actions you want to perform on your AWS resources, you can create your own runbooks\. To create a custom runbook, you can manually create a local YAML or JSON format file with the appropriate automation actions\. Alternatively, you can use Document Builder in the Systems Manager Automation console to build a custom runbook\.

Using Document Builder, you can add automation actions to your custom runbook and provide the required parameters without having to use JSON or YAML syntax\. After you add steps and create the runbook, the system converts the actions you've added into the YAML format that Systems Manager can use to run automation\.

Runbooks support the use of Markdown, a markup language, which allows you to add wiki\-style descriptions to runbooks and individual steps within the runbook\. For more information about using Markdown, see [Using Markdown in AWS](https://docs.aws.amazon.com/general/latest/gr/aws-markdown.html)\.

## Create a runbook using Document Builder<a name="create-runbook"></a>

**Before you begin**  
We recommend that you read about the different actions that you can use within a runbook\. For more information, see [Systems Manager Automation actions reference](automation-actions.md)\.

**To create a runbook using Document Builder**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Create automation**\.

1. For **Name**, enter a descriptive name for the runbook\.

1. For **Document description**, provide the markdown style description for the runbook\. You can provide instructions for using the runbook, numbered steps, or any other type of information to describe the runbook\. Refer to the default text for information about formatting your content\.
**Tip**  
Toggle between **Hide preview** and **Show preview** to see what your description content looks like as you compose\.

1. \(Optional\) For **Assume role**, enter the name or ARN of a service role to perform actions on your behalf\. If you don't specify a role, Automation uses the access permissions of the user who runs the automation\.
**Important**  
For runbooks not owned by Amazon that use the `aws:executeScript` action, a role must be specified\. For information, see [Permissions for using runbooks](automation-document-script-considerations.md#script-permissions)\.

1. \(Optional\) For **Outputs**, enter any outputs for the automation of this runbook to make available for other processes\. 

   For example, if your runbook creates a new AMI, you might specify \["CreateImage\.ImageId"\], and then use this output to create new instances in a subsequent automation\.

1. \(Optional\) Expand the **Input parameters** section and do the following\.

   1. For **Parameter name**, enter a descriptive name for the runbook parameter you're creating\.

   1. For **Type**, choose a type for the parameter, such as String or MapList\.

   1. For **Required**, do one of the following: 
      + Choose **Yes** if a value for this runbook parameter must be supplied at runtime\.
      + Choose **No** if the parameter isn't required, and \(optional\) enter a default parameter value in **Default value**\.

   1. For **Description**, enter a description for the runbook parameter\.
**Note**  
To add more runbook parameters, choose **Add a parameter**\. To remove a runbook parameter, choose the **X** \(Remove\) button\.

1. \(Optional\) Expand the **Target type** section and choose a target type to define the kinds of resources the automation can run on\. For example, to use a runbook on EC2 instances, choose `/AWS::EC2::Instance`\.
**Note**  
If you specify a value of '`/`', the runbook can run on all types of resources\. For a list of valid resource types, see [AWS Resource Types Reference](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html) in the *AWS CloudFormation User Guide*\.

1. \(Optional\) Expand the **Document tags** section and enter one or more tag key\-value pairs to apply to the runbook\. Tags make it easier to identify, organize, and search for resources\. For more information, see [Tagging Systems Manager documents](tagging-documents.md)\.

1. In the **Step 1** section, provide the following information\.
   + For **Step name**, enter a descriptive name for the first step of the automation\.
   + For **Action type**, select the action type to use for this step\.

     For a list and information about the available action types, see [Systems Manager Automation actions reference](automation-actions.md)\.
   + For **Description**, enter a description for the automation step\. You can use Markdown to format your text\.
   + Depending on the **Action type** selected, enter the required inputs for the action type in the **Step inputs** section\. For example, if you selected the action `aws:approve`, you must specify a value for the `Approvers` property\.

     For information about the step input fields, see the entry in [Systems Manager Automation actions reference](automation-actions.md) for the action type you selected\. For example: [`aws:executeStateMachine` – Run an AWS Step Functions state machine](automation-action-executeStateMachine.md)\.
   + \(Optional\) For **Additional inputs**, provide any additional input values needed for your runbook\. The available input types depend on the action type you selected for the step\. \(Note that some action types require input values\.\)
**Note**  
To add more inputs, choose **Add optional input**\. To remove an input, choose the **X** \(Remove\) button\.
   + \(Optional\) For **Outputs**, enter any outputs for this step to make available for other processes\.
**Note**  
**Outputs** isn't available for all action types\.
   + \(Optional\) Expand the **Common properties** section and specify properties for the actions that are common to all automation actions\. For example, for **Timeout seconds**, you can provide a value in seconds to specify how long the step can run before it's stopped\.

     For more information, see [Properties shared by all actions](automation-actions.md#automation-common)\.
**Note**  
To add more steps, select **Add step** and repeat the procedure for creating a step\. To remove a step, choose **Remove step**\.

1. Choose **Create automation** to save the runbook\.

## Create a runbook that runs scripts<a name="create-runbook-scripts"></a>

The following procedure shows how to use Document Builder in the AWS Systems Manager Automation console to create a custom runbook that runs a script\.

The first step of the runbook you create runs a script to launch an Amazon Elastic Compute Cloud \(Amazon EC2\) instance\. The second step runs another script to monitor for the instance status check to change to `ok`\. Then, an overall status of `Success` is reported for the automation\.

**Before you begin**  
Make sure you have completed the following steps:
+ Verify that you have administrator privileges, or that you have been granted the appropriate permissions to access Systems Manager in AWS Identity and Access Management \(IAM\)\. 

  For information, see [ Verifying user access for runbooks](automation-setup.md#automation-setup-user-access)\.
+ Verify that you have an IAM service role for Automation \(also known as an *assume role*\) in your AWS account\. The role is required because this walkthrough uses the `aws:executeScript` action\. 

  For information about creating this role, see [Configuring a service role \(assume role\) access for automations](automation-setup.md#automation-setup-configure-role)\. 

  For information about the IAM service role requirement for running `aws:executeScript`, see [Permissions for using runbooks](automation-document-script-considerations.md#script-permissions)\.
+ Verify that you have permission to launch EC2 instances\. 

  For information, see [IAM and Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/UsingIAM.html#intro-to-iam) in the *Amazon EC2 User Guide for Linux Instances*\.

**To create a custom runbook that runs scripts using Document Builder**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Create automation**\.

1. For **Name**, type this descriptive name for the runbook: **LaunchInstanceAndCheckStatus**\.

1. \(Optional\) For **Document description**, replace the default text with a description for this runbook, using Markdown\. The following is an example\.

   ```
   ##Title: LaunchInstanceAndCheckState
       -----
       **Purpose**: This runbook first launches an EC2 instance using the AMI ID provided in the parameter ```imageId```. The second step of this runbook continuously checks the instance status check value for the launched instance until the status ```ok``` is returned.
       
       ##Parameters:
       -----
       Name | Type | Description | Default Value
       ------------- | ------------- | ------------- | -------------
       assumeRole | String | (Optional) The ARN of the role that allows Automation to perform the actions on your behalf. | -
       imageId  | String | (Optional) The AMI ID to use for launching the instance. The default value uses the latest Amazon Linux AMI ID available. | {{ ssm:/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-gp2 }}
   ```

1. For **Assume role**, enter the ARN of the IAM service role for Automation \(Assume role\) for the automation, in the format **arn:aws:iam::111122223333:role/AutomationServiceRole**\. Substitute your AWS account ID for 111122223333\.

   The role you specify is used to provide the permissions needed to start the automation\.
**Important**  
For runbooks not owned by Amazon that use the `aws:executeScript` action, a role must be specified\. For information, see [Permissions for using runbooks](automation-document-script-considerations.md#script-permissions)\.

1. Expand **Input parameters** and do the following\.

   1. For **Parameter name**, enter **imageId**\.

   1. For **Type**, choose **String**\.

   1. For **Required**, choose No\. 

   1. For **Default value**, enter the following\.

      ```
      {{ ssm:/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-gp2 }}
      ```
**Note**  
This value launches an Amazon EC2 instance using the latest Amazon Linux Amazon Machine Image \(AMI\) ID\. If you want to use a different AMI, replace the value with your AMI ID\.

   1. For **Description**, enter the following\.

      ```
      (Optional) The AMI ID to use for launching the instance. The default value uses the latest released Amazon Linux AMI ID.
      ```

1. Choose **Add a parameter** to create the second parameter, **tagValue**, and enter the following\.

   1. For **Parameter name**, enter **tagValue**\.

   1. For **Type**, choose **String**\.

   1. For **Required**, choose No\. 

   1. For **Default value**, enter **LaunchedBySsmAutomation**\. This adds the tag key\-pair value `Name:LaunchedBySsmAutomation` to the instance\.

   1. For **Description**, enter the following\.

      ```
      (Optional) The tag value to add to the instance. The default value is LaunchedBySsmAutomation.
      ```

1. Choose **Add a parameter** to create the third parameter, **instanceType**, and enter the following information\.

   1. For **Parameter name**, enter **instanceType**\.

   1. For **Type**, choose **String**\.

   1. For **Required**, choose No\. 

   1. For **Default value**, enter **t2\.micro**\.

   1. For **Parameter description**, enter the following\.

      ```
      (Optional) The instance type to use for the instance. The default value is t2.micro.
      ```

1. Expand **Target type** and choose **"/"**\.

1. \(Optional\) Expand **Document tags** to apply resource tags to your runbook\. For **Tag key**, enter **Purpose**, and for **Tag value**, enter **LaunchInstanceAndCheckState**\.

1. In the **Step 1** section, complete the following steps\.

   1. For **Step name**, enter this descriptive step name for the first step of the automation: **LaunchEc2Instance**\.

   1. For **Action type**, choose **Run a script** \(**aws:executeScript**\)\.

   1. For **Description**, enter a description for the automation step, such as the following\.

      ```
      **About This Step**
          
          This step first launches an EC2 instance using the ```aws:executeScript``` action and the provided script.
      ```

   1. Expand **Inputs**\.

   1. For **Runtime**, choose the runtime language to use to run the provided script\.

   1. For **Handler**, enter **launch\_instance**\. This is the function name declared in the following script\.
**Note**  
This is not required for PowerShell\.

   1. For **Script**, replace the default contents with the following\. Be sure to match the script with the corresponding runtime value\.

------
#### [ Python ]

      ```
      def launch_instance(events, context):
            import boto3
            ec2 = boto3.client('ec2')
          
            image_id = events['image_id']
            tag_value = events['tag_value']
            instance_type = events['instance_type']
          
            tag_config = {'ResourceType': 'instance', 'Tags': [{'Key':'Name', 'Value':tag_value}]}
          
            res = ec2.run_instances(ImageId=image_id, InstanceType=instance_type, MaxCount=1, MinCount=1, TagSpecifications=[tag_config])
          
            instance_id = res['Instances'][0]['InstanceId']
          
            print('[INFO] 1 EC2 instance is successfully launched', instance_id)
          
            return { 'InstanceId' : instance_id }
      ```

------
#### [ PowerShell ]

      ```
      Install-Module AWS.Tools.EC2 -Force
          Import-Module AWS.Tools.EC2
          
          $payload = $env:InputPayload | ConvertFrom-Json
          
          $imageid = $payload.image_id
          
          $tagvalue = $payload.tag_value
          
          $instanceType = $payload.instance_type
          
          $type = New-Object Amazon.EC2.InstanceType -ArgumentList $instanceType
          
          $resource = New-Object Amazon.EC2.ResourceType -ArgumentList 'instance'
          
          $tag = @{Key='Name';Value=$tagValue}
          
          $tagSpecs = New-Object Amazon.EC2.Model.TagSpecification
          
          $tagSpecs.ResourceType = $resource
          
          $tagSpecs.Tags.Add($tag)
          
          $res = New-EC2Instance -ImageId $imageId -MinCount 1 -MaxCount 1 -InstanceType $type -TagSpecification $tagSpecs
          
          return @{'InstanceId'=$res.Instances.InstanceId}
      ```

------

   1. Expand **Additional inputs**\. 

   1. For **Input name**, choose **InputPayload**\. For **Input value**, enter the following YAML data\. 

      ```
      image_id: "{{ imageId }}"
          tag_value: "{{ tagValue }}"
          instance_type: "{{ instanceType }}"
      ```

1. Expand **Outputs** and do the following:
   + For **Name**, enter **payload**\.
   + For **Selector**, enter **$\.Payload**\.
   + For **Type**, choose StringMap\.

1. Choose **Add step** to add a second step to the runbook\. The second step queries the status of the instance launched in Step 1 and waits until the status returned is `ok`\.

1. In the **Step 2** section, do the following\.

   1. For **Step name**, enter this descriptive name for the second step of the automation: **WaitForInstanceStatusOk**\.

   1. For **Action type**, choose **Run a script** \(**aws:executeScript**\)\.

   1. For **Description**, enter a description for the automation step, such as the following\.

      ```
      **About This Step**
          
          The script continuously polls the instance status check value for the instance launched in Step 1 until the ```ok``` status is returned.
      ```

   1. For **Runtime**, choose the runtime language to be used for executing the provided script\.

   1. For **Handler**, enter **poll\_instance**\. This is the function name declared in the following script\.
**Note**  
This is not required for PowerShell\.

   1. For **Script**, replace the default contents with the following\. Be sure to match the script with the corresponding runtime value\.

------
#### [ Python ]

      ```
      def poll_instance(events, context):
            import boto3
            import time
          
            ec2 = boto3.client('ec2')
          
            instance_id = events['InstanceId']
          
            print('[INFO] Waiting for instance status check to report ok', instance_id)
          
            instance_status = "null"
          
            while True:
              res = ec2.describe_instance_status(InstanceIds=[instance_id])
          
              if len(res['InstanceStatuses']) == 0:
                print("Instance status information is not available yet")
                time.sleep(5)
                continue
          
              instance_status = res['InstanceStatuses'][0]['InstanceStatus']['Status']
          
              print('[INFO] Polling to get status of the instance', instance_status)
          
              if instance_status == 'ok':
                break
          
              time.sleep(10)
          
            return {'Status': instance_status, 'InstanceId': instance_id}
      ```

------
#### [ PowerShell ]

      ```
          Install-Module AWS.Tools.EC2 -Force
          
          $inputPayload = $env:InputPayload | ConvertFrom-Json
          
          $instanceId = $inputPayload.payload.InstanceId
          
          $status = Get-EC2InstanceStatus -InstanceId $instanceId
          
          while ($status.Status.Status -ne 'ok'){
             Write-Host 'Polling get status of the instance', $instanceId
          
             Start-Sleep -Seconds 5
          
             $status = Get-EC2InstanceStatus -InstanceId $instanceId
          }
          
          return @{Status = $status.Status.Status; InstanceId = $instanceId}
      ```

------

   1. Expand **Additional inputs**\. 

   1. For **Input name**, choose **InputPayload**\. For **Input value**, enter the following:

      ```
      {{ LaunchEc2Instance.payload }}
      ```

1. Choose **Create automation** to save the runbook\.