# Walkthrough: Using Document Builder to create a custom Automation document<a name="automation-walk-document-builder"></a>

The following walkthrough shows how to use Document Builder in the Systems Manager Automation console to create a custom Automation document and then run the custom Automation document\.

The first step of the Automation document you create runs a script to launch an Amazon Elastic Compute Cloud \(EC2\) instance\. The second step runs another script to monitor for the instance status check to change to `ok`\. Then, an overall status of `Success` is reported for the automation execution\.

**Before You Begin**  
Before you begin this walkthrough, do the following: 
+ Verify that you have administrator privileges, or that you have been granted the appropriate permissions to access Systems Manager in AWS Identity and Access Management \(IAM\)\. 

  For information, see [ Verifying user access for Automation workflows](automation-setup.md#automation-setup-user-access)\.
+ Verify that you have an IAM service role for Automation \(also known as an *assume role*\) in your AWS account\. The role is required because this walkthrough uses the **aws:executeScript** action\. 

  For information about creating this role, see [Configuring a service role \(assume role\) access for Automation workflows](automation-setup.md#automation-setup-configure-role)\. 

  For information about the IAM service role requirement for running **aws:executeScript**, see [Permissions for running Automation executions](automation-document-script.md#execution-permissions)\.
+ Verify that you have permission to launch EC2 instances\. 

  For information, see [IAM and Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/UsingIAM.html#intro-to-iam) in the *Amazon EC2 User Guide for Linux Instances*\.

**Topics**
+ [Step 1: Create the custom Automation document](#automation-walk-document-builder-create)
+ [Step 2: Run the custom Automation document](#automation-walk-document-builder-run)

## Step 1: Create the custom Automation document<a name="automation-walk-document-builder-create"></a>

Use the following procedure to create a custom Automation document that launches an EC2 instance and waits for the instance status check to change to `ok`\.

**Tip**  
If you copy and paste values from this walkthrough into Document Builder, such as parameter names and handler names, make sure to delete any leading or trailing spaces added to the text value you enter\.

**To create a custom Automation document using Document Builder**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Create automation**\.

1. For **Name**, type this descriptive name for the document: **LaunchInstanceAndCheckStatus**\.

1. \(Optional\) For **Document description**, replace the default text with a description for this document, using Markdown\. The following is an example\.

   ```
   ##Title: LaunchInstanceAndCheckState
   -----
   **Purpose**: This Automation document first launches an EC2 instance using the AMI ID provided in the parameter ```imageId```. The second step of this document continuously checks the instance status check value for the launched instance until the status ```ok``` is returned.
   
   ##Parameters:
   -----
   Name | Type | Description | Default Value
   ------------- | ------------- | ------------- | -------------
   assumeRole | String | (Optional) The ARN of the role that allows Automation to perform the actions on your behalf. | -
   imageId  | String | (Optional) The AMI ID to use for launching the instance. The default value uses the latest Amazon Linux AMI ID available. | {{ ssm:/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-gp2 }}
   ```

1. For **Assume role**, enter the ARN of the IAM service role for Automation \(Assume role\) for the automation execution, in the format **arn:aws:iam::111122223333:role/AutomationServiceRole**\. Substitute your AWS account ID for 111122223333\.

   The role you specify is used to provide the permissions needed to start the automation execution\.
**Important**  
For Automation documents not owned by Amazon that use the `aws:executeScript` action, a role must be specified\. For information, see [Permissions for running Automation executions](automation-document-script.md#execution-permissions)\.

1. Expand **Input parameters** and do the following\.

   1. For **Parameter name**, enter **imageId**\.

   1. For **Type**, choose **String**\.

   1. For **Required**, choose No\. 

   1. For **Default value**, enter the following\.

      ```
      {{ ssm:/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-gp2 }}
      ```
**Note**  
This value launches an EC2 instance using the latest Amazon Linux Amazon Machine Image \(AMI\) ID\. If you want to use a different AMI, replace the value with your AMI ID\.

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

1. \(Optional\) Expand **Document tags** to apply resource tags to your Automation document\. For **Tag key**, enter **Purpose**, and for **Tag value**, enter **LaunchInstanceAndCheckState**\.

1. In the **Step 1** section, complete the following steps\.

   1. For **Step name**, enter this descriptive step name for the first step of the automation workflow: **LaunchEc2Instance**\.

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

1. Choose **Add step** to add a second step to the Automation document\. The second step queries the status of the instance launched in Step 1 and waits until the status returned is `ok`\.

1. In the **Step 2** section, do the following\.

   1. For **Step name**, enter this descriptive name for the second step of the automation workflow: **WaitForInstanceStatusOk**\.

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

1. Choose **Create automation** to save the document\.

## Step 2: Run the custom Automation document<a name="automation-walk-document-builder-run"></a>

Use the following procedure to run the custom Automation document created in Step 1\. The custom Automation document launches an EC2 instance and waits for the instance check to change to the `ok` status\.

**To run the custom Automation document**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**, and then choose **Execute automation**\.

1. In the **Automation document** list, choose the **Owned by me** tab and then choose the button next to the custom Automation document you created, **LaunchInstanceAndCheckStatus**\.

1. In the **Document details** section, for **Document version**, verify that **Default version at runtime** is selected\.

1. Choose **Next**\.

1. At the top of the **Execute automation document** page, verify that **Simple execution** is selected\.

1. Choose **Execute**\.

1. After both steps in the automation workflow complete, in the **Executed steps** area, choose the step ID of a step to view steps details, including any step output\.
**Note**  
It can take several minutes for the `ok` status to be returned\.

1. \(Optional\) Unless you plan to use the EC2 instance created by this walkthrough for other purposes, you can terminate the instance\. For information, see [Terminate Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

   You can identify the instance by the name **LaunchedBySsmAutomation** that you tagged it with in [Step 1: Create the custom Automation document](#automation-walk-document-builder-create)\.