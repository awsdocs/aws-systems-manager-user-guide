# Viewing inventory history and change tracking<a name="sysman-inventory-history"></a>

You can view AWS Systems Manager Inventory history and change tracking for all of your managed nodes by using [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/)\. AWS Config provides a detailed view of the configuration of AWS resources in your AWS account\. This includes how the resources are related to one another and how they were configured in the past so that you can see how the configurations and relationships change over time\. To view inventory history and change tracking, you must turn on the following resources in AWS Config: 
+ SSM:ManagedInstanceInventory
+ SSM:PatchCompliance
+ SSM:AssociationCompliance
+ SSM:FileData

**Note**  
Note the following important details about Inventory history and change tracking:  
If you use AWS Config to track changes in your system, you must configure Systems Manager Inventory to collect `AWS:File` metadata so that you can view file changes in AWS Config \(`SSM:FileData`\)\. If you don't, then AWS Config doesn't track file changes on your system\.
By turning on SSM:PatchCompliance and SSM:AssociationCompliance, you can view Systems Manager Patch Manager patching and Systems Manager State Manager association compliance history and change tracking\. For more information about compliance management for these resources, see [Working with Compliance](sysman-compliance-about.md)\. 

The following procedure describes how to turn on inventory history and change\-track recording in AWS Config by using the AWS Command Line Interface \(AWS CLI\)\. For more information about how to choose and configure these resources in AWS Config, see [Selecting Which Resources AWS Config Records](https://docs.aws.amazon.com/config/latest/developerguide/select-resources.html) in the *AWS Config Developer Guide*\. For information about AWS Config pricing, see [Pricing](https://aws.amazon.com/config/pricing/)\.

**Before you begin**

AWS Config requires AWS Identity and Access Management \(IAM\) permissions to get configuration details about Systems Manager resources\. In the following procedure, you must specify an Amazon Resource Name \(ARN\) for an IAM role that gives AWS Config permission to Systems Manager resources\. You can attach the `AWS_ConfigRole` managed policy to the IAM role that you assign to AWS Config\. For more information about this role, see [AWS managed policy: AWS\_ConfigRole](https://docs.aws.amazon.com/config/latest/developerguide/security-iam-awsmanpol.html#security-iam-awsmanpol-AWS_ConfigRole) in the *AWS Config Developer Guide*\. For information about how to create an IAM role and assign the `AWS_ConfigRole` managed policy to that role, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\. 

**To turn on inventory history and change\-track recording in AWS Config**

1. Install and configure the AWS Command Line Interface \(AWS CLI\), if you haven't already\.

   For information, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)\.

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

After you configure history and change tracking, you can drill down into the history for a specific managed node by choosing the **AWS Config** button in the Systems Manager console\. You can access the **AWS Config** button from either the **Managed Instances** page or the **Inventory** page\. Depending on your monitor size, you might need to scroll to the right side of the page to see the button\.