# Viewing inventory history and change tracking<a name="sysman-inventory-history"></a>

You can view Systems Manager Inventory history and change tracking for all of your managed instances by using [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/)\. AWS Config provides a detailed view of the configuration of AWS resources in your AWS account\. This includes how the resources are related to one another and how they were configured in the past so that you can see how the configurations and relationships change over time\. To view inventory history and change tracking, you must enable the following resources in AWS Config\. 
+ SSM:ManagedInstanceInventory
+ SSM:PatchCompliance
+ SSM:AssociationCompliance
+ SSM:FileData

**Note**  
By enabling SSM:PatchCompliance and SSM:AssociationCompliance, you can view Patch Manager patching and State Manager association compliance history and change tracking\. For more information about compliance management for these resources, see [Working with Configuration Compliance](sysman-compliance-about.md)\. 

The following procedure describes how to enable inventory history and change\-track recording in AWS Config by using the AWS CLI\. For more information about how to choose and configure these resources in AWS Config, see [Selecting Which Resources AWS Config Records](https://docs.aws.amazon.com/config/latest/developerguide/select-resources.html) in the *AWS Config Developer Guide*\. For information about AWS Config pricing, see [Pricing](https://aws.amazon.com/config/pricing/)\.

**Before You Begin**

AWS Config requires AWS Identity and Access Management \(IAM\) permissions to get configuration details about Systems Manager resources\. In the following procedure, you must specify an Amazon Resource Name \(ARN\) for an IAM role that gives AWS Config permission to Systems Manager resources\. You can attach the **AWSConfigRole** managed policy to the IAM role that you assign to AWS Config\. For information about how to create an IAM role and assign the **AWSConfigRole** managed policy to that role, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\. 

**To enable inventory history and change\-track recording in AWS Config**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Copy and paste the following JSON sample into a simple text file and save it as recordingGroup\.json\.

   ```
   {
      "allSupported":false,
      "includeGlobalResourceTypes":false,
      "resourceTypes":[
         "AWS::SSM::AssociationCompliance",
         "AWS::SSM::PatchCompliance",
         "AWS::SSM::ManagedInstanceInventory",
         "AWS::SSM::FileData"
      ]
   }
   ```

1. Run the following command to load the recordingGroup\.json file into AWS Config\.

   ```
   aws configservice put-configuration-recorder --configuration-recorder name=myRecorder,roleARN=arn:aws:iam::123456789012:role/myConfigRole --recording-group file://recordingGroup.json
   ```

1. Run the following command to start recording inventory history and change tracking\.

   ```
   aws configservice start-configuration-recorder --configuration-recorder-name myRecorder
   ```

After you configure history and change tracking, you can drill down into the history for a specific managed instance by choosing the **AWS Config** button in the Systems Manager console\.

![\[A button in Systems Manager that opens the AWS Config console.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-awsconfig-button.png)

You can access the **AWS Config** button from either the **Managed Instances** page or the **Inventory** page\. Depending on your monitor size, you might need to scroll to the right side of the page to see the button\.